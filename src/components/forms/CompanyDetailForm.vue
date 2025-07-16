<script setup>
import { ref, watch } from 'vue'
import KInput from '../common/KInput.vue'
import KDropdown from '../common/KDropdown.vue'

const fullName = ref('')
const fullNameValidation = ref({})

// Define validation rules for Full Name
const fullNameValidationRule = {
  validate: (value) => {
    if (!value.trim()) {
      return false // Empty or whitespace only
    }
    if (value.toLowerCase() === 'korporatio') {
      return false // Specific disallowed name
    }

    return true // Valid
  },
  message: 'Full name is required and "Korporatio" is not allowed.',
}

const email = ref('')
const emailValidation = ref({})

// Define validation rules for Email
const emailValidationRule = {
  validate: (value) => {
    const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/ // Simple regex
    if (!value.trim()) {
      return false // Empty or whitespace only
    }
    if (value.toLowerCase() === 'korporatio@email.com') {
      return false // Specific disallowed email
    }
    if (!emailRegex.test(value)) {
      return false // Invalid format
    }
    return true // Valid
  },
  message: 'Email is required and must be a valid format. "korporatio@email.com" is not allowed.',
}

const selectedCountry = ref(null) // Initialize with null or a default value from your options

// 3. Define the options array for the dropdown
const countryOptions = ref([
  { value: null, text: 'Please select a country' }, // Optional: a default "select" option
  { value: 'US', text: 'United States' },
  { value: 'CA', text: 'Canada' },
  { value: 'GB', text: 'United Kingdom' },
  { value: 'AU', text: 'Australia' },
  { value: 'TH', text: 'Thailand' },
])

// 4. Define the validation rule for the dropdown
const countryValidationRule = {
  validate: (value) => {
    // A simple validation: ensures a value is selected and not null or an empty string
    return value !== null && value !== ''
  },
  message: 'Please select a country from the list.',
}
</script>

<template>
  <div class="mt-8 font-bold text-white">Point of contact</div>
  <div class="h-0.5 bg-primary-light ms-2"></div>
  <div class="flex flex-row mt-8">
    <div class="text-gray-300 basis-2/6 ms-4 text-sm">
      This is the individual that we will communicate with. Communications related to this form but
      also to the company once incorporated will be sent to the same email address. You can change
      it later on if required.
    </div>
    <div class="flex flex-col basis-3/6 ms-12 text-white">
      <KInput
        id="full-name"
        label="Full Name"
        type="text"
        placeholder="Enter full name"
        v-model="fullName"
        :validation-rule="fullNameValidationRule"
      />

      <KInput
        id="email"
        label="Email"
        type="text"
        placeholder="Enter email"
        v-model="email"
        :validation-rule="emailValidationRule"
        class="mt-4"
      />
    </div>
  </div>

  <!-- Company Information-->
  <div class="mt-8 font-bold text-white">Company Information</div>
  <div class="h-0.5 bg-primary-light ms-2"></div>
  <div class="flex flex-row mt-8">
    <div class="text-gray-300 basis-2/6 ms-4 text-sm">
      Every company must have a name and a designation. For the company name you can use both
      letters and numbers, but not special symbols. For the designations, there is no actual
      different between one or another.
    </div>
    <div class="flex flex-col basis-3/6 ms-12 text-white">
      <KInput
        id="company-name"
        label="Company Name"
        type="text"
        placeholder="The name you want your company to have"
        v-model="fullName"
        :validation-rule="fullNameValidationRule"
      />

      <KInput
        id="email"
        label="Alternative company name"
        type="text"
        placeholder="The name to use if the first name is not available"
        v-model="email"
        :validation-rule="emailValidationRule"
        class="mt-4"
      />
      <!-- 
      <KDropdown
        id="country-selector"
        label="Select Your Country"
        placeholder="Choose a country"
        v-model="selectedCountry"
        :options="countryOptions"
        :validation-rule="countryValidationRule"
        class="mt-4"
      /> -->

      <div class="mt-[300px]" />
    </div>
  </div>
</template>
