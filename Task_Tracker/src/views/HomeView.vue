
<script setup>
import { ref } from 'vue'

import AddTask from '../components/AddTask.vue'
import TaskList from '../components/TaskList.vue'

// Store all tasks
const tasks = ref([])

// Add a new task
function addTask(title) {
  tasks.value.push({
    id: Date.now(),
    title: title,
    date: new Date().toLocaleDateString(),
    status: 'Pending'
  })
}

// Delete a task
function deleteTask(id) {
  tasks.value = tasks.value.filter(task => task.id !== id)
}

// Change task status
function toggleStatus(id) {
  const task = tasks.value.find(task => task.id === id)

  if (task) {
    task.status =
      task.status === 'Pending' ? 'Completed' : 'Pending'
  }
}
</script>

<template>
  <div class="container">

    <h1>
      <span class="task">Task</span>
      <span class="tracker"> Tracker</span>
    </h1>

    <!-- Add Task Component -->
    <AddTask @add-task="addTask" />

    <!-- Task List Component -->
    <TaskList
      :tasks="tasks"
      @delete-task="deleteTask"
      @toggle-status="toggleStatus"
    />

  </div>
</template>

<style scoped>
.container {
  width: 90%;
  max-width: 1000px;
  margin: 50px auto;
  padding: 20px;
}

h1 {
  font-size: 36px;
  font-weight: bold;
  margin-bottom: 30px;
}

.task {
  color: #3b82f6;
}

.tracker {
  color: white;
}
</style>
