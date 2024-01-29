<script>
export default {
  data() {
    return {
      minPasswordLength: 4,
      maxPasswordLength: 20,
      passwordLength: 8,
      checkboxes: {
        includeNumbers: { model: false, label: "Include Numbers" },
        includeUppercase: { model: false, label: "Include Uppercase Letters" },
        includeLowercase: { model: false, label: "Include Lowercase Letters" },
        includeSpecialChars: {
          model: false,
          label: "Include Special Characters",
        },
      },
      generatedPassword: "",
      passwordStrengthIndex: 0,
      strengthSegments: [1, 2, 3, 4], // Number of segments in the strength bar
    };
  },
  methods: {
    generatePassword() {
      const characters = [
        "abcdefghijklmnopqrstuvwxyz",
        "ABCDEFGHIJKLMNOPQRSTUVWXYZ",
        "0123456789",
        "!@#$%^&*()_+[]{}|;:,.<>?",
      ];

      let allChars = characters[0];

      if (this.checkboxes.includeUppercase.model) allChars += characters[1];
      if (this.checkboxes.includeNumbers.model) allChars += characters[2];
      if (this.checkboxes.includeSpecialChars.model) allChars += characters[3];

      let password = "";
      for (let i = 0; i < this.passwordLength; i++) {
        const randomIndex = Math.floor(Math.random() * allChars.length);
        password += allChars[randomIndex];
      }

      this.generatedPassword = password;
      this.calculatePasswordStrength();
    },
    calculatePasswordStrength() {
      if (this.passwordLength < 6) {
        this.passwordStrengthIndex = 1;
      } else {
        let strengthCount = 0;
        strengthCount += this.checkboxes.includeUppercase.model ? 1 : 0;
        strengthCount += this.checkboxes.includeLowercase.model ? 1 : 0;
        strengthCount += this.checkboxes.includeNumbers.model ? 1 : 0;
        strengthCount += this.checkboxes.includeSpecialChars.model ? 1 : 0;

        this.passwordStrengthIndex = strengthCount;
      }
    },
    copyToClipboard() {
      const textarea = document.createElement("textarea");
      textarea.value = this.generatedPassword;
      document.body.appendChild(textarea);
      textarea.select();
      document.execCommand("copy");
      document.body.removeChild(textarea);

      alert("Password copied to clipboard!");
    },
  },
};
</script>

<template>
  <div id="app">
    <div>
      <p>Password: {{ generatedPassword }}</p>
      <button @click="copyToClipboard" v-show="generatedPassword">Copy</button>
    </div>

    <label :for="passwordLength">Password Length: </label>
    <input
      type="range"
      :id="passwordLength"
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
