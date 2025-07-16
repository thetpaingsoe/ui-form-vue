<script setup lang="ts">
import Actions from '@/components/Actions.vue'
import FormHeader from '@/components/FormHeader.vue'
import SectionForm from '@/components/SectionForm.vue'
import SectionNavigation from '@/components/SectionNavigation.vue'
import { ref } from 'vue'

const sections = ['Company Details', 'Shareholders', 'Beneficial Owner', 'Director']
const currentSection = ref(0)

const sectionFormRef = ref()

const formData = ref({
  company_detail: {
    // fullName: 'Jeff',
    // selectedDesignation: 'CEO',
  },
  shareholders: {},
  beneficial_owner: {},
  director: {},
})

function handleSectionChange(index) {
  if (index < 0 || index >= sections.length) return false

  if (index < currentSection.value) {
    currentSection.value = index
  }

  if (sectionFormRef?.value?.validate()) {
    currentSection.value = index
  }
}

function handleNext() {}
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
    <SectionForm :currentSection="currentSection" v-model="formData" ref="sectionFormRef" />

    <!-- Actions -->
    <Actions
      @next="handleSectionChange"
      :currentSection="currentSection"
      @back="handleSectionChange"
    />
  </main>
</template>
