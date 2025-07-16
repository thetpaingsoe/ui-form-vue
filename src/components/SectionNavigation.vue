<script setup>
defineProps({
  sections: Array,
  currentSection: Number,
})

defineEmits(['navigate'])
</script>
<template>
  <div class="flex gap-6 mt-12 items-center">
    <div
      v-for="(section, index) in sections"
      :key="index"
      class="flex items-center cursor-pointer"
      @click="$emit('navigate', index)"
    >
      <div
        class="rounded-full w-12 h-12 flex items-center justify-center text-white border-8 transition-all duration-300"
        :class="{
          'bg-primary-light border-primary-light': index === currentSection,
          ' border-gray-500 bg-gray-500': index !== currentSection,
          'opacity-70': index !== currentSection,
        }"
      >
        {{ index + 1 }}
      </div>
      <div
        class="ms-2 transition-all"
        :class="index === currentSection ? 'text-primary-light ' : 'text-gray-500'"
      >
        {{ section }}
      </div>
    </div>
  </div>
</template>

<!-- <template>
  <div class="flex text-primary-light items-center mt-12">
    <div
      class="text-white border-8 border-primary-light bg-primary-light rounded-full w-12 h-12 text-center pt-1"
    >
      1
    </div>
    <div class="ms-2 font-bold">Section Navigation</div>
  </div>
</template> -->
