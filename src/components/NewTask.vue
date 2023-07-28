<template>
  <div class="new-task-card">
  <h1>Add a new Task</h1>
  <div v-if="showErrorMessage">
      <p class="error-text">{{ errorMessage }}</p>
  </div>
  <div class="input-fields">
      <div class="input-field-home">
          <input class="input-title" type="text" placeholder="Add a Task Title" v-model="name">
      </div>
      <div class="input-field-home">
          <input class="input-text" type="text" placeholder="Add a Task Description" v-model="description">
      </div>
      <button @click="addTask" class="add-task">Add your task</button>
  </div>
</div>
</template>

<script setup>
import { ref, onMounted, defineEmits } from "vue";
import { useTaskStore } from "../stores/task"
import { supabase } from "../supabase";
import { useUserStore } from "../stores/user";


const taskStore = useTaskStore();

// variables para los valors de los inputs

const name = ref('');
const description = ref('');

// constant to save a variable that holds an initial false boolean value for the errorMessage container that is conditionally displayed depending if the input field is empty
const showErrorMessage = ref(false);

// const constant to save a variable that holds the value of the error message
const errorMessage = ref(null);

// const tasks = ref([]);

// Declarar el evento que el componente puede emitir
const emits = defineEmits(['nueva-tarea-creada']);

// Arrow function para crear tareas.
const addTask = async () => {
  try {
    // Verificar que los campos no estén vacíos
    if (name.value.length === 0 || description.value.length === 0) {
      // Primero comprobamos que ningún campo del input esté vacío y lanzamos el error con un timeout para informar al usuario.
      showErrorMessage.value = true;
      errorMessage.value = 'The task title or description is empty';
      setTimeout(() => {
        showErrorMessage.value = false;
      }, 5000);
      return; // Salimos de la función si hay campos vacíos
    }

    // Aquí mandamos los valores a la store para crear la nueva Task.

    const response = await taskStore.addTask(name.value, description.value);
    console.log(response); // Agregar esta línea para obtener más detalles de la respuesta

    if (response) {
      const nuevaTarea = response;
      // Emitir el evento con la nueva tarea creada
      emits('nueva-tarea-creada', nuevaTarea);

      // Limpiar los campos del formulario después de crear la tarea
      name.value = '';
      description.value = '';
    } else {
      console.error('Error al crear la tarea: Respuesta inválida');
    }
  } catch (error) {
    console.error('Error al crear la tarea:', error.message);
  }
};

</script>

<style>
h1{
  font-family: Verdana, Geneva, Tahoma, sans-serif;
  padding-left: 7.5%;
  font-size: 1.25rem;
  color: #593639
}

h2 {
  font-family: Verdana, Geneva, Tahoma, sans-serif;
  font-size: 1.5rem;
}

.error-text {
  font-family: Verdana, Geneva, Tahoma, sans-serif;
  padding-left: 7.5%;
  padding-top: 2%;
  color: #CB7B7A;
}

.new-task-card {
  margin-left: 7%;
  margin-right: 7%;
  padding-top: 3.5%;
  margin-bottom: 3%;
  padding-bottom: 0%;
  background-color: rgba(203, 123, 122, 0.3);
}

.input-field-home input {
  background-color: #F2F2F2;
  color: black;
  min-width: 72%;
  border-radius: 10%;
  margin-left: 7.5%;
  margin-top: 2%;
}
.input-title {
  height: 10px;
}
.input-text {
  height: 90px;
}

.input-title::placeholder {
  font-family: Verdana, Geneva, Tahoma, sans-serif;
  color: #8C8C8C;
  font-size: 0.75rem;
}

.input-text::placeholder {
  font-family: Verdana, Geneva, Tahoma, sans-serif;
  color: #8C8C8C;
  font-size: 0.75rem;
}

.add-task {
  font-family: Helvetica, Arial;
  font-size: 1rem;
  font-weight: 600;
  color: white;
  background-color: #A67772;
  padding: 1.5%;
  border: none;
  border-radius: 5%;
  margin: 3% 0% 3% 80%;
}

.add-task:hover {
cursor: pointer;
}

@media (max-width: 768px){
h1{
  font-family: Verdana, Geneva, Tahoma, sans-serif;
  padding-left: 7.5%;
  font-size: 1rem;
}

h2 {
  font-family: Verdana, Geneva, Tahoma, sans-serif;
  font-size: 1.5rem;
}

.new-task-card {
  margin-left: 3.5%;
  margin-right: 3.5%;
  padding-top: 3%;
  padding-bottom: 3%;
  margin-bottom: 3%;
  background-color: rgba(203, 123, 122, 0.3);
}

.error-text {
  font-family: Verdana, Geneva, Tahoma, sans-serif;
  padding-left: 7.5%;
  padding-top: 2%;
  color: #CB7B7A;
}

.input-field-home input {
  background-color: #F2F2F2;
  color: black;
  min-width: none;
  border-radius: 8%;
  margin-left: 7.5%;
}

.input-title {
  height: 10px;
}

.input-text {
  height: 70px;
}

.input-title::placeholder {
  font-family: Verdana, Geneva, Tahoma, sans-serif;
  color: #81848C;
}

.input-text::placeholder {
  font-family: Verdana, Geneva, Tahoma, sans-serif;
  color: #81848C;
}

.add-task {
  font-family: Helvetica, Arial;
  font-size: 0.75rem;
  font-weight: 600;
  color: white;
  background-color: #A67772;
  padding: 3%;
  border: none;
  border-radius: 8%;
  margin: 3% 0% 0% 65%;
}

.add-task:hover {
cursor: pointer;
}

}
</style>
