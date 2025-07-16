<script setup>
import { ref } from 'vue'
import KInput from '../common/KInput.vue'
import KDropdown from '../common/KDropdown.vue'
import KMultiSelectDropdown from '../common/KMultiSelectDropdown.vue'
import KNumberInput from '../common/KNumberInput.vue'

// Full Name
const fullName = ref('')
const fullNameValidationRule = {
  validate: (value) => {
    if (!value.trim()) {
      return false // Empty or whitespace only
    }
    if (value.toLowerCase() === 'korporatio') {
      return false // Specific disallowed name
    }

    return true
  },
  message: 'Full name is required and "Korporatio" is not allowed.',
}

// Email
const email = ref('')
const emailValidationRule = {
  validate: (value) => {
    const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/
    if (!value.trim()) {
      return false // Empty or whitespace only
    }
    if (value.toLowerCase() === 'korporatio@email.com') {
      return false // Specific disallowed email
    }
    if (!emailRegex.test(value)) {
      return false // Invalid format
    }
    return true
  },
  message: 'Email is required and must be a valid format. "korporatio@email.com" is not allowed.',
}

// Company Name
const companyName = ref('')
const companyNameValidateionState = ref({})
const companyNameValidationRule = {
  validate: (value) => {
    if (!value.trim()) {
      return false // Empty or whitespace only
    }
    if (value.toLowerCase() === 'korporatio') {
      return false // Specific disallowed name
    }

    return true
  },
  message: 'Company name is required and "Korporatio" is not allowed.',
}

// Alternative Company Name
const altCompanyName = ref('')
const altCompanyNameValidationRule = {
  validate: (value) => {
    if (!value.trim()) {
      return false // Empty or whitespace only
    }
    if (value.toLowerCase() === 'korporatio') {
      return false // Specific disallowed name
    }

    return true
  },
  message: 'Alternative Company name is required and "Korporatio" is not allowed.',
}

// Company Designation
const selectedDesignation = ref(null)
const designationOptions = ref([
  { value: null, text: 'Select the option that you prefer' },
  { value: 'CEO', text: 'Chief Executive Officer' },
  { value: 'COO', text: 'Chief Operating Officer' },
  { value: 'CFO', text: 'Chief Financial Officer' },
  { value: 'CTO', text: 'Chief Technology Officer' },
  { value: 'CMO', text: 'Chief Marketing Officer' },
])

const designationValidationRule = {
  validate: (value) => {
    return value !== null && value !== ''
  },
  message: 'Please select a designation from the list.',
}

// Jurisdiction of operation
const selectedOperationCountry = ref(null)
const operationCountryOptions = ref([
  { value: null, text: 'Select the country where you are located' },
  { value: 'SG', text: 'Singapore' },
  { value: 'US', text: 'United States' },
  { value: 'CA', text: 'Canada' },
  { value: 'GB', text: 'United Kingdom' },
  { value: 'AU', text: 'Australia' },
  { value: 'TH', text: 'Thailand' },
])
const operationCountryValidationRule = {
  validate: (value) => {
    return value !== null && value !== ''
  },
  message: 'Please select a operation country from the list.',
}

// Target Jurisdictions
const selectedTargetCountry = ref([])
const targetCountryOptions = [
  { value: 'SG', text: 'Singapore' },
  { value: 'US', text: 'United States' },
  { value: 'CA', text: 'Canada' },
  { value: 'GB', text: 'United Kingdom' },
  { value: 'AU', text: 'Australia' },
  { value: 'TH', text: 'Thailand' },
]
const targetCountryValidation = {
  validate: (val) => val.length >= 1 && val.length <= 3,
  message: 'Please select 1 to 3 target jurisdictions',
}

// Number of shares
const numOfShares = ref('')
const numOfSharesValidationRule = {
  validate: (value) => {
    if (!value.trim()) {
      return false // Empty or whitespace only
    }
    if (value < 1) {
      return false // Specific disallowed name
    }

    return true
  },
  message: 'Please enter number of shares at least 1 share.',
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
    <div class="flex flex-col basis-4/7 ms-12 text-white">
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
    <div class="flex flex-col basis-4/7 ms-12 text-white">
      <KInput
        id="company-name"
        label="Company Name"
        type="text"
        placeholder="The name you want your company to have"
        v-model="companyName"
        :validation-rule="companyNameValidationRule"
        :validation-state="companyNameValidateionState"
      />

      <KInput
        id="email"
        label="Alternative company name"
        type="text"
        placeholder="The name to use if the first name is not available"
        v-model="altCompanyName"
        :validation-rule="altCompanyNameValidationRule"
        class="mt-4"
      />

      <KDropdown
        id="company-designation"
        cid="company-designation"
        label="Company designation"
        placeholder="Select the option that you prefer"
        v-model="selectedDesignation"
        :options="designationOptions"
        :validation-rule="designationValidationRule"
        class="mt-4"
      />
    </div>
  </div>

  <!-- Countries of interest -->
  <div class="mt-8 font-bold text-white">Countries of interest</div>
  <div class="h-0.5 bg-primary-light ms-2"></div>
  <div class="flex flex-row mt-8">
    <div class="text-gray-300 basis-2/6 ms-4 text-sm">
      We are required to check that the company will not be interacting with forbidden locations.
      For 'jurisdiction of operation' if you are alone, select your country of residency. If you
      have more shareholders, pick the most relevant. For 'target jurisdiction' select 1-3 countries
      that are relevant. Even if you will have clients from other counties, it's ok.
    </div>
    <div class="flex flex-col basis-4/7 ms-12 text-white">
      <KDropdown
        id="operation-country"
        cid="operation-country"
        label="Jurisdiction of operation"
        placeholder="Select the country where you are located"
        v-model="selectedOperationCountry"
        :options="operationCountryOptions"
        :validation-rule="operationCountryValidationRule"
        class="mt-4"
      />

      <KMultiSelectDropdown
        id="hobbies"
        cid="hobbies"
        label="Select your hobbies"
        v-model="selectedTargetCountry"
        :options="targetCountryOptions"
        :validation-rule="targetCountryValidation"
        placeholder="Select the countries where your clients are located"
        class="mt-4"
      />
    </div>
  </div>

  <!-- Shares structure -->
  <div class="mt-8 font-bold text-white">Shares structure</div>
  <div class="h-0.5 bg-primary-light ms-2"></div>
  <div class="flex flex-row mt-8">
    <div class="text-gray-300 basis-2/6 ms-4 text-sm">
      All companies must have at least 1 share. Apart from that, you can structure things in
      whatever way you like. Issued shares are shares that will be distributed from day 1. Unissued
      shares are shares that you can distribute later on, i.e. to future team members or investors.
      The value per shares represents your personal liabiity. So, if you wish to reduce risks, just
      pick the smallest number.
    </div>
    <div class="flex flex-col basis-4/7 ms-12 text-white">
      <KNumberInput
        id="num-of-shares"
        label="Number of Shares"
        type="text"
        placeholder="Select how many shares you wish to have"
        v-model="numOfShares"
        :validation-rule="numOfSharesValidationRule"
        class="mt-4"
      />
    </div>
  </div>
  <div class="mt-[300px]" />
</template>
