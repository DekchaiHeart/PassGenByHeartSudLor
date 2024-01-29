<script>
import { ref, computed, watch } from "vue";

export default {
  setup() {
    // State
    const minPasswordLength = 4;
    const maxPasswordLength = 20;

    const passwordLength = ref(8);
    const checkboxes = {
      includeNumbers: { model: ref(false), label: "Include Numbers" },
      includeUppercase: {
        model: ref(false),
        label: "Include Uppercase Letters",
      },
      includeLowercase: {
        model: ref(false),
        label: "Include Lowercase Letters",
      },
      includeSpecialChars: {
        model: ref(false),
        label: "Include Special Characters",
      },
    };

    const generatedPassword = ref("");
    const passwordStrengthIndex = ref(0);
    const strengthSegments = [1, 2, 3, 4];

    // Computed
    const computedId = computed(() => `passwordLength-${passwordLength.value}`);

    // Watchers
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

    // Methods
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

      alert("Password copied to clipboard!");
    };

    return {
      minPasswordLength,
      maxPasswordLength,
      passwordLength,
      checkboxes,
      generatedPassword,
      passwordStrengthIndex,
      strengthSegments,
      computedId,
      generatePassword,
      copyToClipboard,
    };
  },
};
</script>

<template>
  <div id="app">
    <div>
      <p>Password: {{ generatedPassword }}</p>
      <button @click="copyToClipboard" v-if="generatedPassword">Copy</button>
    </div>

    <label :for="computedId">Password Length: </label>
    <input
      type="range"
      :id="computedId"
      v-model="passwordLength"
      :min="minPasswordLength"
      :max="maxPasswordLength"
    />
    {{ passwordLength }}

    <div v-for="(checkbox, label) in checkboxes" :key="label">
      <input type="checkbox" :id="label" v-model="checkbox.model" />
      <label :for="label">{{ checkbox.label }}</label>
    </div>

    <div class="strength-bar">
      <div
        v-for="(segment, index) in strengthSegments"
        :key="index"
        class="bar-segment"
        :class="{ selected: index < passwordStrengthIndex }"
      ></div>
    </div>

    <button @click="generatePassword" v-show="!generatedPassword">
      Generate Password
    </button>
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
