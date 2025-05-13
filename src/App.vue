<script setup>
import { onMounted, ref } from "vue";

const name = ref("John Doe");
const status = ref("active");
const tasks = ref(["Task One", "Task Two", "Task Three"]);

const toggleStatus = () => {
  if (status.value === "active") {
    status.value = "pending";
  } else if (status.value === "pending") {
    status.value = "inactive";
  } else {
    status.value = "active";
  }
};
const newTask = ref("");
const addTask = () => {
  if (newTask.value.trim() !== "") {
    tasks.value.push(newTask.value);
    newTask.value = "";
  }
};

const deleteTask = (index) => {
  tasks.value.splice(index, 1);
};

onMounted(async () => {
  try {
    const response = await fetch("https://jsonplaceholder.typicode.com/todos");
    const data = await response.json();
    tasks.value = data.map((task) => task.title);
  } catch (error) {
    console.error("Error fetching tasks:", error);
  }
});
</script>

<template>
  <h1>Hello {{ name }}</h1>
  <br />
  <p v-if="status === 'active'">User is Active</p>
  <p v-else-if="status === 'pending'">User is Pending</p>
  <p v-else>User is Inactive</p>

  <form @submit.prevent="addTask">
    <label for="newTask">Add Task</label>
    <input
      type="text"
      v-model="newTask"
      placeholder="Enter your task"
      id="newTask"
    />
    <button type="submit">Submit</button>
  </form>
  <br />
  <ul>
    <li v-for="(task, index) in tasks" :key="task">
      <span>{{ task }}</span>
      <button @click="deleteTask(index)">X</button>
    </li>
  </ul>

  <button @click="toggleStatus">Change status</button>
</template>
