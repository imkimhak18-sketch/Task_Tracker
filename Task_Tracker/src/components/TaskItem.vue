
<script setup>
defineProps({
  task: {
    type: Object,
    required: true
  },
  index: {
    type: Number,
    required: true
  }
})

// Events sent to TaskList
defineEmits(['delete-task', 'toggle-status'])
</script>

<template>
  <tr>

    <td>{{ index + 1 }}</td>

    <td>{{ task.title }}</td>

    <td>{{ task.date }}</td>

    <td>
      <button
        class="status-button"
        :class="task.status === 'Pending' ? 'pending' : 'completed'"
        @click="$emit('toggle-status', task.id)"
      >
        {{ task.status }}
      </button>
    </td>

    <td>
      <button
        class="delete-button"
        @click="$emit('delete-task', task.id)"
      >
        Delete
      </button>
    </td>

  </tr>
</template>

<style scoped>
.status-button {
  border: none;
  padding: 7px 14px;
  border-radius: 5px;
  cursor: pointer;
  font-weight: bold;
}

.pending {
  background-color: #422006;
  color: #fbbf24;
}

.completed {
  background-color: #14532d;
  color: #4ade80;
}

.delete-button {
  background-color: #dc2626;
  color: white;
  padding: 7px 12px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.delete-button:hover {
  background-color: #b91c1c;
}
</style>
