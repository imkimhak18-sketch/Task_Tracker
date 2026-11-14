
<script setup>
import TaskItem from './TaskItem.vue'

// Receive tasks from HomeView
defineProps({
  tasks: {
    type: Array,
    required: true
  }
})

// Events that this component can send to its parent
defineEmits(['delete-task', 'toggle-status', 'update-task'])
</script>

<template>
  <div class="table-container">

    <table class="task-table">

      <thead>
        <tr>
          <th>No.</th>
          <th>Task Name</th>
          <th>Date Created</th>
          <th>Status</th>
          <th>Action</th>
        </tr>
      </thead>

      <tbody>
        <TaskItem
          v-for="(task, index) in tasks"
          :key="task.id"
          :task="task"
          :index="index"
          @delete-task="$emit('delete-task', $event)"
          @toggle-status="$emit('toggle-status', $event)"
          @update-task="$emit('update-task', $event)"
        />
      </tbody>

    </table>

    <p v-if="tasks.length === 0" class="empty-message">
      No tasks yet. Add your first task!
    </p>

  </div>
</template>

<style scoped>
.table-container {
  width: 100%;
  overflow-x: auto;
}

.task-table {
  width: 100%;
  table-layout: fixed;
  border-collapse: collapse;
  margin-top: 20px;
  color: white;
}

.task-table th,
.task-table td {
  padding: 15px 10px;
  text-align: center;
  border-bottom: 1px solid #333;
  overflow-wrap: anywhere;
}

.task-table th {
  background-color: #1e293b;
  color: #3b82f6;
}

.task-table td {
  background-color: #111827;
}

.task-table th:nth-child(1) {
  width: 8%;
}

.task-table th:nth-child(2) {
  width: 32%;
}

.task-table th:nth-child(3) {
  width: 22%;
}

.task-table th:nth-child(4) {
  width: 18%;
}

.task-table th:nth-child(5) {
  width: 20%;
}

.empty-message {
  color: #9ca3af;
  text-align: center;
  margin-top: 20px;
}
</style>
