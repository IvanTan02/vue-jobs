<script setup>
import { ref, onMounted } from 'vue';

const name = ref('John Smith');
const status = ref(true);
const tasks = ref(['Task 1', 'Task 2', 'Task 3']);
const newTask = ref('');

const toggleStatus = () => {
  status.value = !status.value;
};

const addTask = () => {
  if (newTask.value.trim() !== '') {
    tasks.value.push(newTask.value);
    newTask.value = '';
  }
}

const deleteTask = (index) => {
  tasks.value.splice(index, 1);
}

onMounted(async () => {
  try {
    const response = await fetch('https://jsonplaceholder.typicode.com/todos');
    const data = await response.json();
    tasks.value = data.map((task) => task.title);
  } catch (error) {
    console.error(error);
  }
});

</script>

<template>
  <h1>{{ name }}</h1>
  <p v-if="status">User is Active</p>
  <p v-else="status">User is Inactive</p>

  <form @submit.prevent="addTask">
    <label for="newTask">Add Task</label>
    <input type="text" name="newTask" id="newTask" v-model="newTask" placeholder="New Task">
    <button type="submit">Add</button>
  </form>

  <h3>Tasks:</h3>
  <ul>
    <li v-for="(task, index) in tasks" :key="task">
      <span>{{ task }}</span>
      <button @click="deleteTask(index)">Delete</button>
    </li>
  </ul>

  <button v-on:click="toggleStatus">Change Status</button>
</template>
