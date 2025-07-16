<template>
  <div class="flex justify-between mt-20 mb-30">
    <button
      class="bg-gray-200 hover:bg-gray-300 px-4 py-2 rounded"
      :disabled="currentSection === 0"
      @click="$emit('prev')"
    >
      Previous
    </button>

    <div class="flex gap-4">
      <button
        v-if="currentSection < totalSections - 1"
        class="bg-primary text-white px-6 py-2 rounded hover:bg-primary-light"
        @click="$emit('next')"
      >
        Next
      </button>
      <button
        v-else
        class="bg-green-600 text-white px-6 py-2 rounded hover:bg-green-700"
        @click="$emit('save')"
      >
        Save
      </button>
    </div>
  </div>
</template>

<script setup>
defineProps({
  currentSection: Number,
  totalSections: Number,
})

defineEmits(['prev', 'next', 'save'])
</script>
