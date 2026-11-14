<script setup>
import { ref, nextTick } from 'vue'

const props = defineProps({
  task: {
    type: Object,
    required: true
  },
  index: {
    type: Number,
    required: true
  }
})

// Events sent to TaskList (new: update-task)
const emit = defineEmits(['delete-task', 'toggle-status', 'update-task'])

// Is this row being edited right now?
const isEditing = ref(false)

// The text inside the edit input
const editText = ref('')

// Reference to the input element so we can focus it
const editInput = ref(null)

async function startEditing() {
  editText.value = props.task.title
  isEditing.value = true

  // Wait for Vue to show the input, then focus it
  await nextTick()
  editInput.value.focus()
}

function saveEdit() {
  // Already finished (prevents saving twice)
  if (!isEditing.value) return

  const newTitle = editText.value.trim()

  // Only send an update if it is not empty and actually changed
  if (newTitle !== '' && newTitle !== props.task.title) {
    emit('update-task', { id: props.task.id, title: newTitle })
  }

  isEditing.value = false
}

function cancelEdit() {
  isEditing.value = false
}
</script>

<template>
  <tr>

    <td>{{ index + 1 }}</td>

    <td>
      <!-- Edit mode -->
      <input
        v-if="isEditing"
        ref="editInput"
        v-model="editText"
        class="edit-input"
        @keyup.enter="saveEdit"
        @keyup.esc="cancelEdit"
        @blur="saveEdit"
      />

      <!-- Normal mode -->
      <span
        v-else
        class="task-title"
        title="Double-click to edit"
        @dblclick="startEditing"
      >
        {{ task.title }}
      </span>
    </td>

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
.task-title {
  cursor: pointer;
}

.edit-input {
  width: 90%;
  padding: 8px;
  border: 1px solid #3b82f6;
  border-radius: 6px;
  background-color: #1f2937;
  color: white;
  outline: none;
  text-align: center;
}

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