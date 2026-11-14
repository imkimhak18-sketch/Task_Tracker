<script setup>
defineProps({
  currentFilter: {
    type: String,
    required: true
  },
  counts: {
    type: Object,
    required: true
  }
})

// Tell the parent which filter was clicked
defineEmits(['change-filter'])
</script>

<template>
  <div class="filter-bar">

    <button
      :class="{ active: currentFilter === 'All' }"
      @click="$emit('change-filter', 'All')"
    >
      All ({{ counts.all }})
    </button>

    <button
      :class="{ active: currentFilter === 'Pending' }"
      @click="$emit('change-filter', 'Pending')"
    >
      Pending ({{ counts.pending }})
    </button>

    <button
      :class="{ active: currentFilter === 'Completed' }"
      @click="$emit('change-filter', 'Completed')"
    >
      Completed ({{ counts.completed }})
    </button>

  </div>
</template>

<style scoped>
.filter-bar {
  display: flex;
  gap: 10px;
  margin-top: 20px;
}

button {
  padding: 8px 16px;
  background-color: #1f2937;
  color: #9ca3af;
  border: 1px solid #374151;
  border-radius: 6px;
  cursor: pointer;
}

button:hover {
  background-color: #374151;
}

/* The selected filter */
button.active {
  background-color: #2563eb;
  border-color: #2563eb;
  color: white;
}
</style>