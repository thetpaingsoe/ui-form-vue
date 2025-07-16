<script setup>
import { computed } from 'vue'
import CompanyDetailForm from './forms/CompanyDetailForm.vue'

const props = defineProps({
  currentSection: Number,
  modelValue: Object,
})

const emit = defineEmits(['update:modelValue'])

const components = [CompanyDetailForm, CompanyDetailForm, CompanyDetailForm]
const sectionKeyMap = ['company_detail', 'shareholders', 'beneficial_owner', 'director']

const currentComponent = computed(() => components[props.currentSection])
const sectionKey = computed(() => sectionKeyMap[props.currentSection])
</script>

<template>
  <component :is="currentComponent" v-model="modelValue[sectionKey]"> </component>
</template>
