<script setup>
import { computed, ref } from 'vue'
import CompanyDetailForm from './forms/CompanyDetailForm.vue'
import Sharedholders from './forms/Sharedholders.vue'
import BeneficialOwner from './forms/BeneficialOwner.vue'
import Director from './forms/Director.vue'

const props = defineProps({
  currentSection: Number,
  modelValue: Object,
})

const childRef = ref()
const emit = defineEmits(['update:modelValue'])

const components = [CompanyDetailForm, Sharedholders, BeneficialOwner, Director]
const sectionKeyMap = ['company_detail', 'shareholders', 'beneficial_owner', 'director']

const currentComponent = computed(() => components[props.currentSection])
const sectionKey = computed(() => sectionKeyMap[props.currentSection])

function validate() {
  return childRef?.value?.validate()
}

// Expose it to parent
defineExpose({ validate })
</script>

<template>
  <component :is="currentComponent" v-model="modelValue[sectionKey]" ref="childRef" />
</template>
