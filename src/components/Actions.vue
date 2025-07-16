<template>
  <div class="flex justify-between mt-20 mb-30">
    <button
      class="bg-primary-dark border-half-px border-gray-200 text-white rounded-sm hover:bg-gray-800 py-2 px-8"
      :disabled="currentSection === 0"
      @click="$emit('back', currentSection - 1)"
    >
      <i class="me-2 fa-solid fa-arrow-left-long"></i>
      BACK
    </button>

    <button
      class="bg-primary-dark border-half-px border-gray-200 text-white rounded-sm hover:bg-gray-800 py-2 px-8 cursor-pointer"
      @click="$emit('save')"
    >
      <span class="text-[11px] text-gray-400">
        I know the form autosaved but,<br /><span class="font-bold text-[13px] text-white"
          >SAVE & EXIT</span
        >
        i wish to anyways. ;)</span
      >
    </button>

    <button
      id="nextForm"
      class="bg-primary text-white rounded-sm hover:bg-primary-light py-2 px-8 cursor-pointer"
      @click="$emit('next', currentSection + 1)"
    >
      NEXT
      <i class="ms-2 fa-solid fa-arrow-right-long"></i>
    </button>
  </div>
</template>

<script setup>
defineProps({
  currentSection: Number,
  totalSections: Number,
})

defineEmits(['back', 'next', 'save'])
</script>
