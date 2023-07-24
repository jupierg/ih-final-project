<template>
  <h1>Add a new Task</h1>
  <div v-if="showErrorMessage">
      <p class="error-text">{{ errorMessage }}</p>
  </div>
  <div class="input-fields">
      <div class="input-field-home">
          <input type="text" placeholder="Add a Task Title" v-model="name">
      </div>
      <div class="input-field-home">
          <input type="text" placeholder="Add a Task Description" v-model="description">
      </div>
      <button @click="addTask" class="add-task">Add your task</button>
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
@media (max-width: 768px){
h1{
  font-family: Verdana, Geneva, Tahoma, sans-serif;
  padding-top: 1.5rem;
  padding-left: 1.5rem;
  font-size: 1rem;
}

h2 {
  font-family: Verdana, Geneva, Tahoma, sans-serif;
  font-size: 1.5rem;
};

.input-field-home {
  padding-left: 1.5rem;
};

.add-task:hover {
cursor: pointer;
};

button {
  color: blue;
  border: none;
}
}
</style>
