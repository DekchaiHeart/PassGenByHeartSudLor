<script setup>
import { ref, computed, watch } from "vue";

const minPasswordLength = 4;
const maxPasswordLength = 20;

const passwordLength = ref(8);
const checkboxes = {
  includeNumbers: {
    model: ref(false),
    label: "Include Numbers",
  },
  includeUppercase: {
    model: ref(false),
    label: "Include Uppercase",
  },
  includeLowercase: {
    model: ref(false),
    label: "Include Lowercase",
  },
  includeSpecialChars: {
    model: ref(false),
    label: "Include Symbols",
  },
};

const generatedPassword = ref("");
const passwordStrengthIndex = ref(0);
const strengthSegments = [1, 2, 3, 4];

// Computed
const computedId = computed(() => `passwordLength-${passwordLength.value}`);

watch(
  [
    passwordLength,
    checkboxes.includeUppercase.model,
    checkboxes.includeLowercase.model,
    checkboxes.includeNumbers.model,
    checkboxes.includeSpecialChars.model,
  ],
  () => {
    generatePassword();
  }
);

const generatePassword = () => {
  const characters = [
    "abcdefghijklmnopqrstuvwxyz",
    "ABCDEFGHIJKLMNOPQRSTUVWXYZ",
    "0123456789",
    "!@#$%^&*()_+[]{}|;:,.<>?",
  ];

  let allChars = characters[0];

  if (checkboxes.includeUppercase.model.value) allChars += characters[1];
  if (checkboxes.includeNumbers.model.value) allChars += characters[2];
  if (checkboxes.includeSpecialChars.model.value) allChars += characters[3];

  let password = "";
  for (let i = 0; i < passwordLength.value; i++) {
    const randomIndex = Math.floor(Math.random() * allChars.length);
    password += allChars[randomIndex];
  }

  generatedPassword.value = password;
  calculatePasswordStrength();
};

const calculatePasswordStrength = () => {
  if (passwordLength.value < 6) {
    passwordStrengthIndex.value = 1;
  } else {
    let strengthCount = 0;
    strengthCount += checkboxes.includeUppercase.model.value ? 1 : 0;
    strengthCount += checkboxes.includeLowercase.model.value ? 1 : 0;
    strengthCount += checkboxes.includeNumbers.model.value ? 1 : 0;
    strengthCount += checkboxes.includeSpecialChars.model.value ? 1 : 0;

    passwordStrengthIndex.value = strengthCount;
  }
};

const copyToClipboard = () => {
  const textarea = document.createElement("textarea");
  textarea.value = generatedPassword.value;
  document.body.appendChild(textarea);
  textarea.select();
  document.execCommand("copy");
  document.body.removeChild(textarea);

  alert("Copied to clipboard!");
};
</script>

<template>
  <A href="./"></A>
  <div id="app">
    <div class="drawer">
      <input id="my-drawer" type="checkbox" class="drawer-toggle" />
      <div class="drawer-content">
        <!-- Page content here -->
        <label for="my-drawer" class="btn btn-primary drawer-button"
          ><NavButton
        /></label>
      </div>
      <div class="drawer-side">
        <label
          for="my-drawer"
          aria-label="close sidebar"
          class="drawer-overlay"
        ></label>
        <ul class="menu p-4 w-80 min-h-full bg-base-200 text-base-content">
          <!-- Sidebar content here -->
          <li><a>Sidebar Item 1</a></li>
          <li><a>Sidebar Item 2</a></li>
        </ul>
      </div>
    </div>
    <!-- Theme-Swap -->
    <label class="swap swap-rotate">
      <input type="checkbox" class="theme-controller" value="dark" />
      <!-- sun icon -->
      <svg
        class="swap-on fill-current w-10 h-10"
        xmlns="http://www.w3.org/2000/svg"
        viewBox="0 0 24 24"
      >
        <path
          d="M5.64,17l-.71.71a1,1,0,0,0,0,1.41,1,1,0,0,0,1.41,0l.71-.71A1,1,0,0,0,5.64,17ZM5,12a1,1,0,0,0-1-1H3a1,1,0,0,0,0,2H4A1,1,0,0,0,5,12Zm7-7a1,1,0,0,0,1-1V3a1,1,0,0,0-2,0V4A1,1,0,0,0,12,5ZM5.64,7.05a1,1,0,0,0,.7.29,1,1,0,0,0,.71-.29,1,1,0,0,0,0-1.41l-.71-.71A1,1,0,0,0,4.93,6.34Zm12,.29a1,1,0,0,0,.7-.29l.71-.71a1,1,0,1,0-1.41-1.41L17,5.64a1,1,0,0,0,0,1.41A1,1,0,0,0,17.66,7.34ZM21,11H20a1,1,0,0,0,0,2h1a1,1,0,0,0,0-2Zm-9,8a1,1,0,0,0-1,1v1a1,1,0,0,0,2,0V20A1,1,0,0,0,12,19ZM18.36,17A1,1,0,0,0,17,18.36l.71.71a1,1,0,0,0,1.41,0,1,1,0,0,0,0-1.41ZM12,6.5A5.5,5.5,0,1,0,17.5,12,5.51,5.51,0,0,0,12,6.5Zm0,9A3.5,3.5,0,1,1,15.5,12,3.5,3.5,0,0,1,12,15.5Z"
        />
      </svg>
      <!-- moon icon -->
      <svg
        class="swap-off fill-current w-10 h-10"
        xmlns="http://www.w3.org/2000/svg"
        viewBox="0 0 24 24"
      >
        <path
          d="M21.64,13a1,1,0,0,0-1.05-.14,8.05,8.05,0,0,1-3.37.73A8.15,8.15,0,0,1,9.08,5.49a8.59,8.59,0,0,1,.25-2A1,1,0,0,0,8,2.36,10.14,10.14,0,1,0,22,14.05,1,1,0,0,0,21.64,13Zm-9.5,6.69A8.14,8.14,0,0,1,7.08,5.22v.27A10.15,10.15,0,0,0,17.22,15.63a9.79,9.79,0,0,0,2.1-.22A8.11,8.11,0,0,1,12.14,19.73Z"
        />
      </svg>
    </label>

    <!-- ฟังชั่น -->
    <div>
      <p>Password: {{ generatedPassword }}</p>
      <button @click="copyToClipboard" v-if="generatedPassword">Copy</button>
    </div>

    <!-- ความยาวรหัส -->
    <label :for="computedId">Password Length: </label>
    <input
      type="range"
      :id="computedId"
      v-model="passwordLength"
      :min="minPasswordLength"
      :max="maxPasswordLength"
    />
    {{ passwordLength }}

    <!-- เช็คบ็อกตามความต้องการ -->
    <div
      class="form-control flex items-center p-2"
      v-for="(checkbox, label) in checkboxes"
      :key="label"
    >
      <input
        class="checkbox checkbox-info"
        type="checkbox"
        :id="label"
        v-model="checkbox.model"
      />
      <label class="cursor-pointer label ml-2" :for="label">{{
        checkbox.label
      }}</label>
    </div>

    <!-- เกรดความแข็งแรงรหัส -->
    <div class="strength-bar">
      <div
        v-for="(segment, index) in strengthSegments"
        :key="index"
        class="bar-segment"
        :class="{ selected: index < passwordStrengthIndex }"
      ></div>
    </div>

    <!-- ปุ่มกดสุ่มรหัส -->
    <button @click="generatePassword">Generate Password</button>
  </div>
</template>

<style scoped>
.strength-bar {
  display: flex;
  height: 20px;
  margin-top: 10px;
}

.bar-segment {
  flex: 1;
  background-color: #e0e0e0;
  margin-right: 2px;
}

.bar-segment.selected {
  background-color: #51baee;
}
</style>
