<template>
  <div class="flex flex-col">
    <label :for="id" class="text-white font-medium mb-1">{{ label }}</label>
    <div class="relative">
      <!-- Dropdown display area (mimics an input) -->
      <div
        :id="id"
        @click="toggleDropdown"
        @blur="markAsTouched"
        tabindex="0"
        class="mt-1 block w-full rounded-md px-4 py-2 pr-10 cursor-pointer focus:outline-none focus:ring-2 sm:text-sm"
        :class="[
          touched && !validationState.status
            ? 'border-red-500 focus:border-red-500 focus:ring-red-200 text-red-700'
            : 'border-gray-300 focus:border-blue-500 focus:ring-blue-200 text-white',
          // Add background color for the dropdown display area
          'bg-primary-dark',
        ]"
      >
        {{ selectedOptionText || placeholder }}
      </div>

      <!-- Dropdown Arrow Icon -->
      <span
        class="absolute right-4 top-1/2 -translate-y-1/2 text-lg text-gray-400 pointer-events-none mt-1"
      >
        <i :class="isOpen ? 'fa-solid fa-caret-up' : 'fa-solid fa-caret-down'"></i>
      </span>

      <!-- Validation Icon -->
      <!-- <span
        v-if="touched && modelValue !== ''"
        :class="[
          'absolute right-3 top-1/2 -translate-y-1/2 text-lg',
          validationState.status ? 'text-green-500' : 'text-red-500',
        ]"
      >
        <i :class="validationState.status ? 'fas fa-check-circle' : 'fas fa-times-circle'"></i>
      </span> -->

      <!-- Dropdown Options List -->
      <ul
        v-if="isOpen"
        class="absolute z-10 mt-1 w-full bg-primary-dark border border-gray-300 rounded-md shadow-lg max-h-60 overflow-auto focus:outline-none"
        role="listbox"
      >
        <li
          v-for="option in options"
          :key="option.value"
          @click="selectOption(option.value)"
          class="px-4 py-2 cursor-pointer hover:bg-primary-light hover:text-blue-700 text-gray-800"
          :class="{ 'bg-primary-light text-blue-700': modelValue === option.value }"
          role="option"
          :aria-selected="modelValue === option.value"
        >
          {{ option.text }}
        </li>
      </ul>
    </div>

    <!-- Error Message -->
    <p v-if="touched && !validationState.status" class="mt-1 text-sm text-red-600">
      {{ validationState.message }}
    </p>
  </div>
</template>

<script setup>
import { ref, watch, computed, defineProps, defineEmits, onMounted, onUnmounted } from 'vue'

const props = defineProps({
  modelValue: {
    type: [String, Number, null], // Allow null for initial state
    default: null,
  },
  id: {
    type: String,
    required: true,
  },
  label: {
    type: String,
    required: true,
  },
  placeholder: {
    type: String,
    default: 'Select an option',
  },
  options: {
    type: Array, // Expected format: [{ value: '...', text: '...' }]
    required: true,
  },
  validationRule: {
    type: Object,
    default: () => ({
      validate: () => true,
      message: '',
    }),
  },
})

const emit = defineEmits(['update:modelValue'])

const isOpen = ref(false)
const touched = ref(false)

// Computed property to display the text of the selected option
const selectedOptionText = computed(() => {
  const selected = props.options.find((option) => option.value === props.modelValue)
  return selected ? selected.text : ''
})

// Computed property for validation state
const validationState = computed(() => {
  const isValid = props.validationRule.validate(props.modelValue)
  return {
    status: isValid,
    message: isValid ? '' : props.validationRule.message,
  }
})

// Method to toggle dropdown visibility
const toggleDropdown = () => {
  isOpen.value = !isOpen.value
  if (isOpen.value) {
    // When opening, mark as touched if not already
    touched.value = true
  }
}

// Method to select an option
const selectOption = (value) => {
  emit('update:modelValue', value)
  isOpen.value = false // Close dropdown after selection
  touched.value = true // Mark as touched on selection
}

// Method to mark the dropdown as touched (on blur)
const markAsTouched = () => {
  // Delay setting touched to allow click event on options to fire first
  setTimeout(() => {
    if (!isOpen.value) {
      // Only mark as touched if dropdown is closed
      touched.value = true
    }
  }, 100)
}

// Close dropdown when clicking outside
const handleClickOutside = (event) => {
  const dropdownElement = document.getElementById(props.id)?.closest('.relative')
  if (dropdownElement && !dropdownElement.contains(event.target)) {
    isOpen.value = false
    touched.value = true // Mark as touched when clicking outside
  }
}

onMounted(() => {
  document.addEventListener('click', handleClickOutside)
})

onUnmounted(() => {
  document.removeEventListener('click', handleClickOutside)
})

// Reset touched state if modelValue becomes null/empty after having content
watch(
  () => props.modelValue,
  (newVal) => {
    if ((newVal === null || newVal === '') && touched.value) {
      touched.value = false
    }
  },
)
</script>
