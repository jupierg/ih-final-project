<template>
  <div class="container">
    <h3 :class="['task-added', { crossed: task.is_complete }]">{{ editedTitle || task.title }}</h3>
    <h3 :class="['task-added', { crossed: task.is_complete }]">{{ editedDescription || task.description }}</h3>
    <button @click="deleteTask"> x </button>
    <button :class="{ crossed: task.is_complete }" @click="completeToggle">
      ✓
    </button>
    <button @click="editToggle">Edit</button>
    <div v-if="updateInput && task.id === selectedTaskId">
      <input type="text" v-model="editedTitle" placeholder="New title" />
      <input type="text" v-model="editedDescription" placeholder="New description" />
      <button @click="updateTask">Update</button>
      <p v-if="error" class="error-message">{{ error }}</p>
    </div>
  </div>
</template>

<script setup>
import { ref, defineProps, defineEmits, onUpdated } from 'vue';
import { useTaskStore } from "../stores/task";
import { supabase } from "../supabase";

const taskStore = useTaskStore();

const editedTitle = ref('');
const editedDescription = ref('');
const updateInput = ref(false);
const error = ref('');
const selectedTaskId = ref(null);

const props = defineProps({
  task: Object,
});

const emits = defineEmits(['update-task', 'delete-task', 'toggle-complete']);

const deleteTask = async () => {
  await taskStore.deleteTask(props.task.id);
  emits['delete-task'](props.task.id);
};

const completeToggle = () => {
  props.task.is_complete = !props.task.is_complete;
  taskStore.completeTask(props.task.id, !props.task.is_complete);
};

const editToggle = () => {
  updateInput.value = !updateInput.value;
  selectedTaskId.value = props.task.id;
  editedTitle.value = '';
  editedDescription.value = '';
};

const updateTask = async () => {
  if (!editedTitle.value.trim() && !editedDescription.value.trim()) {
    error.value = 'Task title or description cannot be empty.';
    return;
  }

  error.value = '';
  updateInput.value = false;

  await taskStore.editTask(props.task.id, {
    title: editedTitle.value,
    description: editedDescription.value,
  });

  emits['update-task']({
    id: props.task.id,
    title: editedTitle.value,
    description: editedDescription.value,
  });

  editedTitle.value = '';
  editedDescription.value = '';
};
</script>

<style>
.crossed {
  text-decoration: line-through;
}
.container {
  background-color: bisque;
}

.task-added {
  background-color: grey;
  color: blue;
}
</style>



<!--
**Hints**
1. ref() or reactive() can be used here to store the following, think if you want to store them either individually or
like an object, up to you.

2. Data properties need here are the following: a boolean to store a false**Important variable, a string to store an error,
a string to store the value of the task that the user can edit, an initial false boolean to hide the inputFIeld used to edit
the new task detail or details[this is in reference of the task title and the task description].

3. Store the custom emit events that will be used to call the functions of the homeView for editing, deleting and toggling the
status[completed, not complted] of the taskItem.

4. Function to handle the error with the data properties used to control the error and the the boolean controlling the boolean
empty variable.

5. Function to handle the edit dialogue where the inputField is displayed and the string used to store the value of the
inputField will be used here to save the value as a prop on this function.

6. Function to emmit a custom event emit() that takes 2 parameters a name for the custom event and the value that will be
send via the prop to the parent component. This function can control the toggle completion of the task on the homeview.

7. Function to edit the task information that you decided that the user can edit. This function's body takes in a conditional
that first checks if the value of the task [either title and description or just title] is empty which if true it runs the
function used to handle the error on hint4. Else, the conditional sets the first mentioned boolean data property on hint2
back to its inital boolean value to hide the error message and repeat the same for the data property mentioned 4th on hint2;
a constant that stores an object that holds the oldValue from the prop item and the value of the task coming from the data
property mentioned 3rd on hint2; a custom event emit() that takes 2 parameters a name for the custom event and the value
from the object used on this part of the conditional and lastly this part of the conditional sets the value of input field
to an empty string to clear it from the ui.

8. Function to emmit a custom event emit() that takes 2 parameters a name for the custom event and the value that will be
send via the prop to the parent component. This function can control the removal of  the task on the homeview.
-->
