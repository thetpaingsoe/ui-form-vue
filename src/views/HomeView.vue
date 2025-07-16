<script setup lang="ts">
import Actions from '@/components/Actions.vue'
import FormHeader from '@/components/FormHeader.vue'
import SectionForm from '@/components/SectionForm.vue'
import SectionNavigation from '@/components/SectionNavigation.vue'
import { ref } from 'vue'

const sections = ['Company Details', 'Shareholders', 'Beneficial Owner', 'Director']
const currentSection = ref(0)

const formData = ref({
  basic: {},
  details: {},
  summary: {},
})

function handleSectionChange(index) {
  currentSection.value = index
}
</script>

<template>
  <main class="flex flex-col bg-primary-dark">
    <!-- Form Header -->
    <FormHeader />

    <!-- Section Navigation -->
    <SectionNavigation
      :sections="sections"
      :current-section="currentSection"
      @navigate="handleSectionChange"
    />

    <!-- Form Detail Screen -->
    <SectionForm :currentSection="0" />

    <!-- Actions -->
    <Actions />
  </main>
</template>
