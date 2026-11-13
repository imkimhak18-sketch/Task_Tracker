
<script setup>
import { ref } from 'vue'

const newTask = ref('')
const tasks = ref([])

// Add a new task
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

// Delete a task
function deleteTask(taskId) {
  tasks.value = tasks.value.filter(task => task.id !== taskId)
}

// Change task status
function toggleTaskStatus(task) {
  if (task.status === 'Pending') {
    task.status = 'Completed'
  } else {
    task.status = 'Pending'
  }
}
</script>

<template>
  <div class="welcome">
    <h1>Task Tracker</h1>
    <p>Manage your tasks and organize your day.</p>

    <h2>Add a New Task</h2>

    <form @submit.prevent="addTask">
      <input
        v-model="newTask"
        placeholder="Enter a new task"
      />
      <button type="submit">Add Task</button>
    </form>

    <h2>My Tasks</h2>

    <div
      class="Tasks"
      v-for="task in tasks"
      :key="task.id"
    >
      <p>
        {{ task.title }}

        <span
          :class="task.status === 'Pending' ? 'pending' : 'completed'"
        >
          {{ task.status }}
        </span>

        <button
          class="status-button"
          @click="toggleTaskStatus(task)"
        >
          {{ task.status === 'Pending' ? 'Complete' : 'Pending' }}
        </button>

        <button
          class="delete-button"
          @click="deleteTask(task.id)"
        >
          Delete
        </button>
      </p>
    </div>

    <p v-if="tasks.length === 0">
      No tasks yet. Add your first task!
    </p>
  </div>
</template>

<style scoped>
.welcome {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  min-height: 100vh;
  padding: 20px;
}

h1 {
  color: #212542;
}

p {
  font-size: 1.1rem;
  color: #ae4646;
  padding: 0.5rem 0;
}

h2 {
  margin-top: 1rem;
  font-size: 1.5rem;
}

input {
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
}

button {
  color: white;
  border: none;
  padding: 0.5rem 1rem;
  margin-left: 0.5rem;
  border-radius: 5px;
  cursor: pointer;
}

button:hover {
  opacity: 0.85;
}

.Tasks {
  margin-top: 0.5rem;
}

.pending {
  color: #d97706;
  font-weight: bold;
  margin-left: 10px;
}

.completed {
  color: #16a34a;
  font-weight: bold;
  margin-left: 10px;
}

.status-button {
  background-color: #2563eb;
}

.delete-button {
  background-color: #dc2626;
}
</style>
