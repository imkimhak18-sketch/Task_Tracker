<script setup>
import { ref } from 'vue'

const newTask = ref('')
const dueDate = ref('')   // format: YYYY-MM-DD, or '' if not chosen

// Send the task (title + due date) to the parent
const emit = defineEmits(['add-task'])

function submitTask() {
  if (newTask.value.trim() === '') {
    return
  }

  emit('add-task', {
    title: newTask.value.trim(),
    dueDate: dueDate.value
  })

  // Clear the inputs after adding
  newTask.value = ''
  dueDate.value = ''
}
</script>

<template>
  <div class="add-task">

    <input
      v-model="newTask"
      type="text"
      placeholder="Enter your task..."
      @keyup.enter="submitTask"
    />

    <input
      v-model="dueDate"
      type="date"
      class="date-input"
      title="Due date (optional)"
      @keyup.enter="submitTask"
    />

    <button @click="submitTask">
      Add Task
    </button>

  </div>
</template>

<style scoped>
.add-task {
  display: flex;
  gap: 10px;
  width: 100%;
}

input {
  flex: 1;
  min-width: 0;
  padding: 12px;
  border: 1px solid #374151;
  border-radius: 6px;
  background-color: #1f2937;
  color: white;
  outline: none;
}

.date-input {
  flex: none;
  width: 160px;
  color-scheme: dark; /* makes the calendar popup dark */
}

button {
  padding: 12px 20px;
  background-color: #2563eb;
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
}

button:hover {
  background-color: #1d4ed8;
}
</style>