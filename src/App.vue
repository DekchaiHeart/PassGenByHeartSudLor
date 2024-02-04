<script setup>
import { ref, computed, watch } from "vue";
import NavButton from "./assets/NavButton.vue";
import CopyIcon from "./assets/CopyIcon.vue";
import ExclamationIcon from "./assets/ExclamationIcon.vue";
import Popup from "./assets/Popup.vue";

let error = true;
const borderError = 'class="" ';
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
const passwordStrengthText = ref("Password Strength");
const strengthSegments = [1, 2, 3, 4];

const computedId = computed(() => `passwordLength-${passwordLength.value}`);
const passwordHistory = ref([]);

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

let selectedPasswordLength = passwordLength.value;

const handlePasswordLengthChange = (event) => {
  selectedPasswordLength = parseInt(event.target.value);
};

const generatePassword = () => {
  const characters = [
    "abcdefghijklmnopqrstuvwxyz",
    "ABCDEFGHIJKLMNOPQRSTUVWXYZ",
    "0123456789",
    "!@#$%^&*()_+[]{}|;:,.<>?",
  ];

  let allChars = "";

  if (checkboxes.includeLowercase.model.value) allChars += characters[0];
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
  addToHistory();
};

const addToHistory = () => {
  if (generatedPassword.value) {
    const historyItem = {
      number: passwordHistory.length + 1,
      password: generatedPassword.value,
      length: selectedPasswordLength,
      strength: passwordStrengthIndex.value,
    };

    passwordHistory.value.unshift(historyItem);
  }
};
let strengthCount = 0;
const calculatePasswordStrength = () => {
  strengthCount = 0;
  if (passwordLength.value < 6) {
    passwordStrengthIndex.value = 1;
    passwordStrengthText.value = "Weak";
  } else {
    strengthCount += checkboxes.includeUppercase.model.value ? 1 : 0;
    strengthCount += checkboxes.includeLowercase.model.value ? 1 : 0;
    strengthCount += checkboxes.includeNumbers.model.value ? 1 : 0;
    strengthCount += checkboxes.includeSpecialChars.model.value ? 1 : 0;

    switch (strengthCount) {
      case 1:
        passwordStrengthIndex.value = 1;
        passwordStrengthText.value = "Weak";
        break;
      case 2:
        passwordStrengthIndex.value = 2;
        passwordStrengthText.value = "Medium";
        break;
      case 3:
        passwordStrengthIndex.value = 3;
        passwordStrengthText.value = "Strong";
        break;
      case 4:
        passwordStrengthIndex.value = 4;
        passwordStrengthText.value = "Very Strong";
        break;
      default:
        passwordStrengthIndex.value = 0;
        passwordStrengthText.value = "Password Strength";
        error = true;
        break;
    }
  }
  error = strengthCount === 0;
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
  <div
    id="app"
    class="w-full min-h-screen flex flex-col text-neutral justify-center max-w-[30rem] mx-auto px-4 md:px-0"
  >
    <div id="top" class="flex justify-between space-x-4 space-x-reverse">
      <div class="flex mb-11">
        <h2 class="flex items-center text-2xl font-bold lg:text-3xl">SECURE</h2>
        <Popup />
      </div>
      <div class="flex">
        <!-- Theme-Swap -->
        <label class="swap swap-rotate">
          <input type="checkbox" class="theme-controller" value="dark" />
          <!-- sun icon -->
          <svg
            class="swap-on fill-current w-8 h-8"
            xmlns="http://www.w3.org/2000/svg"
            viewBox="0 0 24 24"
          >
            <path
              d="M5.64,17l-.71.71a1,1,0,0,0,0,1.41,1,1,0,0,0,1.41,0l.71-.71A1,1,0,0,0,5.64,17ZM5,12a1,1,0,0,0-1-1H3a1,1,0,0,0,0,2H4A1,1,0,0,0,5,12Zm7-7a1,1,0,0,0,1-1V3a1,1,0,0,0-2,0V4A1,1,0,0,0,12,5ZM5.64,7.05a1,1,0,0,0,.7.29,1,1,0,0,0,.71-.29,1,1,0,0,0,0-1.41l-.71-.71A1,1,0,0,0,4.93,6.34Zm12,.29a1,1,0,0,0,.7-.29l.71-.71a1,1,0,1,0-1.41-1.41L17,5.64a1,1,0,0,0,0,1.41A1,1,0,0,0,17.66,7.34ZM21,11H20a1,1,0,0,0,0,2h1a1,1,0,0,0,0-2Zm-9,8a1,1,0,0,0-1,1v1a1,1,0,0,0,2,0V20A1,1,0,0,0,12,19ZM18.36,17A1,1,0,0,0,17,18.36l.71.71a1,1,0,0,0,1.41,0,1,1,0,0,0,0-1.41ZM12,6.5A5.5,5.5,0,1,0,17.5,12,5.51,5.51,0,0,0,12,6.5Zm0,9A3.5,3.5,0,1,1,15.5,12,3.5,3.5,0,0,1,12,15.5Z"
            />
          </svg>
          <!-- moon icon -->
          <svg
            class="swap-off fill-current w-8 h-8"
            xmlns="http://www.w3.org/2000/svg"
            viewBox="0 0 24 24"
          >
            <path
              d="M21.64,13a1,1,0,0,0-1.05-.14,8.05,8.05,0,0,1-3.37.73A8.15,8.15,0,0,1,9.08,5.49a8.59,8.59,0,0,1,.25-2A1,1,0,0,0,8,2.36,10.14,10.14,0,1,0,22,14.05,1,1,0,0,0,21.64,13Zm-9.5,6.69A8.14,8.14,0,0,1,7.08,5.22v.27A10.15,10.15,0,0,0,17.22,15.63a9.79,9.79,0,0,0,2.1-.22A8.11,8.11,0,0,1,12.14,19.73Z"
            />
          </svg>
        </label>
        <!-- ปุ่ม History -->
        <div class="drawer">
          <input id="my-drawer" type="checkbox" class="drawer-toggle" />
          <div class="drawer-content">
            <label for="my-drawer" class="drawer-button"><NavButton /></label>
          </div>
          <!-- แถบ History -->
          <div class="drawer-side">
            <label
              for="my-drawer"
              aria-label="close sidebar"
              class="drawer-overlay"
            ></label>
            <ul class="menu p-4 w-80 min-h-full bg-base-200 text-base-content">
              <table>
                <tr>
                  <h2>Password History</h2>
                </tr>
                <tr>
                  <th>No.</th>
                  <th>Password</th>
                  <th>Length</th>
                  <th>Strength</th>
                </tr>
                <tr>
                  <td>
                    <li
                      v-for="historyItem in passwordHistory"
                      :key="historyItem.number"
                    >
                      <p>{{ historyItem.number }}</p>
                    </li>
                  </td>
                  <td>
                    <li
                      v-for="historyItem in passwordHistory"
                      :key="historyItem.number"
                    >
                      <p>{{ historyItem.password }}</p>
                    </li>
                  </td>
                  <td>
                    <li
                      v-for="historyItem in passwordHistory"
                      :key="historyItem.number"
                    >
                      <p>{{ historyItem.length }}</p>
                    </li>
                  </td>
                  <td>
                    <li
                      v-for="historyItem in passwordHistory"
                      :key="historyItem.number"
                    >
                      <p>{{ historyItem.strength }}/4</p>
                    </li>
                  </td>
                </tr>
              </table>
            </ul>
          </div>
        </div>
      </div>
    </div>
    <!-- ฟังชั่น -->
    <div class="flex items-center justify-between p-4 mb-5 bg-accent text-2xl">
      <p v-if="generatedPassword">{{ generatedPassword }}</p>
      <p v-else>P4SSWORD</p>
      <button class="" @click="copyToClipboard">
        <CopyIcon />
      </button>
    </div>

    <div class="bg-accent p-5 rounded-lg">
      <div class="flex items-center text-3xl uppercase">
        <div
          class="z-0 tooltip color-tooltip"
          data-tip="Create a secure password for you"
        >
          <ExclamationIcon />
        </div>
        <h1>Password Generator</h1>
      </div>
      <!-- ความยาวรหัส -->
      <div class="flex items-center justify-between text-xl">
        <label :for="computedId"><p class="text-l">Password Length:</p> </label>
        <p class="text-primary text-3xl">{{ passwordLength }}</p>
      </div>
      <input
        type="range"
        :id="computedId"
        v-model="passwordLength"
        :min="8"
        :max="20"
        class="block mb-2 text-sm font-medium text-gray-900 cursor-pointer dark:text-white"
      />

      <!-- เช็คบ็อกตามความต้องการ -->
      <div
        class="flex items-center p-2"
        v-for="(checkbox, label) in checkboxes"
        :key="label"
      >
        <input
          class="checkbox checkbox-info"
          type="checkbox"
          :id="label"
          v-model="checkbox.model.value"
        />
        <label class="cursor-pointer label ml-2" :for="label">{{
          checkbox.label
        }}</label>
      </div>

      <!-- เกรดความแข็งแรงรหัส -->
      <div class="flex bg-primary p-3">
        <p class="font-bold text-base-100">{{ passwordStrengthText }}</p>

        <div class="strength-bar flex justify-end">
          <div
            v-for="(segment, index) in strengthSegments"
            :key="index"
            class="bar-segment"
            :class="{ selected: index < passwordStrengthIndex }"
          ></div>
        </div>
      </div>
      <!-- โชว์ Error -->
      <div v-if="(error = true)" class="my-1 dark:text-error">
        <p>Please select at least one option.</p>
      </div>
      <!-- ปุ่มกดสุ่มรหัส -->
      <button
        class="w-full btn btn-primary text-base-100"
        @click="generatePassword"
      >
        Generate Password
      </button>
    </div>
  </div>
</template>

<style scoped>
#app {
  font-family: Oswald;
}
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
