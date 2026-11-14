<script setup>
import { ref, computed } from 'vue'

import AddTask from '../components/AddTask.vue'
import TaskFilter from '../components/TaskFilter.vue'
import TaskList from '../components/TaskList.vue'

// Store all tasks
const tasks = ref([])

// Which filter is selected: 'All', 'Pending' or 'Completed'
const currentFilter = ref('All')

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

// Change the selected filter
function setFilter(filter) {
  currentFilter.value = filter
}

// The tasks that match the selected filter
const filteredTasks = computed(() => {
  if (currentFilter.value === 'All') {
    return tasks.value
  }

  return tasks.value.filter(task => task.status === currentFilter.value)
})

// Numbers shown on the filter buttons
const counts = computed(() => ({
  all: tasks.value.length,
  pending: tasks.value.filter(task => task.status === 'Pending').length,
  completed: tasks.value.filter(task => task.status === 'Completed').length
}))
</script>

<template>
  <div class="container">

    <h1>
      <span class="task">Task</span>
      <span class="tracker"> Tracker</span>
    </h1>

    <AddTask @add-task="addTask" />

    <!-- NEW: filter buttons -->
    <TaskFilter
      :current-filter="currentFilter"
      :counts="counts"
      @change-filter="setFilter"
    />

    <!-- CHANGED: send filteredTasks instead of tasks -->
    <TaskList
      :tasks="filteredTasks"
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
