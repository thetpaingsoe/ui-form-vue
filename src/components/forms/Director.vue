<script setup>
function validate() {
  var status = true

  return status
}

// Expose it to parent
defineExpose({ validate })
</script>

<template>
  <div class="mt-8 font-bold text-white">Director</div>
  <div class="h-0.5 bg-primary-light ms-2"></div>
</template>
