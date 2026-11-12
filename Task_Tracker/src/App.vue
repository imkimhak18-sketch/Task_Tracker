<script setup> 

import { ref } from 'vue'
const newTask = ref('')
const tasks = ref([])
function addTask() {
  if (newTask.value.trim() !== '') {
    tasks.value.push({
      id: Date.now(),
      title: newTask.value.trim(),
      status: 'Pending'
    })

    newTask.value = ''
  }
}
function deleteTask(taskId) {
  tasks.value = tasks.value.filter(task => task.id !== taskId)
}


</script> 
<template>
  <div class="welcome">
    <h1>Task Tracker</h1>
    <p> Example of the next feature </p> 
    <h2> Add a new task </h2>
    <form @submit.prevent="addTask">
      <input v-model="newTask" placeholder="Enter a new task" />
      <button type="submit">Add Task</button>
    </form>
    <h2>My Tasks</h2> 
    <div v-for="task in tasks" :key="task.id"> 
      <p> {{ task.title}} {{ task.status }} <button @click="deleteTask(task.id)">Delete</button> </p> 
    </div> 

  </div>
</template>


<style scoped> 
.welcome {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
}
p {
  font-size: 1.2rem;
  color: #333;
  padding: 0.5rem 0;
}
h2 {
  margin-top: 1rem;
  font-size: 1.5rem;
}

</style>