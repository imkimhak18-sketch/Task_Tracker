<script setup>
import { ref, computed, nextTick } from 'vue'

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

const emit = defineEmits(['delete-task', 'toggle-status', 'update-task'])

/* ---------- Edit feature ---------- */
const isEditing = ref(false)
const editText = ref('')
const editInput = ref(null)

async function startEditing() {
  editText.value = props.task.title
  isEditing.value = true

  await nextTick()
  editInput.value.focus()
}

function saveEdit() {
  if (!isEditing.value) return

  const newTitle = editText.value.trim()

  if (newTitle !== '' && newTitle !== props.task.title) {
    emit('update-task', { id: props.task.id, title: newTitle })
  }

  isEditing.value = false
}

function cancelEdit() {
  isEditing.value = false
}

/* ---------- Due date feature ---------- */

// Today's date as text: YYYY-MM-DD (using the local time zone)
function getToday() {
  const now = new Date()
  const year = now.getFullYear()
  const month = String(now.getMonth() + 1).padStart(2, '0')
  const day = String(now.getDate()).padStart(2, '0')
  return `${year}-${month}-${day}`
}

// 'none' | 'normal' | 'today' | 'overdue'
const dueState = computed(() => {
  const due = props.task.dueDate

  // No due date (also covers old tasks saved before this feature)
  if (!due) return 'none'

  // Completed tasks are never overdue
  if (props.task.status === 'Completed') return 'normal'

  const today = getToday()

  if (due < today) return 'overdue'
  if (due === today) return 'today'
  return 'normal'
})

// Show the due date like the "Date Created" column: MM/DD/YYYY
const formattedDueDate = computed(() => {
  const due = props.task.dueDate
  if (!due) return ''

  const [year, month, day] = due.split('-')
  return `${month}/${day}/${year}`
})
</script>

<template>
  <tr>

    <td>{{ index + 1 }}</td>

    <td>
      <input
        v-if="isEditing"
        ref="editInput"
        v-model="editText"
        class="edit-input"
        @keyup.enter="saveEdit"
        @keyup.esc="cancelEdit"
        @blur="saveEdit"
      />

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

    <!-- NEW: Due date cell -->
    <td>
      <span v-if="dueState === 'none'" class="no-due">
        No due date
      </span>

      <div v-else :class="['due', dueState]">
        <span>{{ formattedDueDate }}</span>
        <small v-if="dueState === 'overdue'">Overdue</small>
        <small v-else-if="dueState === 'today'">Due today</small>
      </div>
    </td>

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

/* Due date styles */
.no-due {
  color: #6b7280;
  font-size: 14px;
}

.due {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 2px;
}

.due small {
  font-size: 12px;
  font-weight: bold;
}

.due.overdue {
  color: #f87171;
}

.due.today {
  color: #fbbf24;
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