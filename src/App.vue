<script>
export default {
  data() {
    return {
      passwordLength: 8,
      includeNumbers: false,
      includeUppercase: false,
      includeLowercase: false,
      includeSpecialChars: false,
      generatedPassword: "",
      passwordStrength: "Easy",
    };
  },
  methods: {
    generatePassword() {
      // Generate password based on settings
      const characters = [
        "abcdefghijklmnopqrstuvwxyz",
        "ABCDEFGHIJKLMNOPQRSTUVWXYZ",
        "0123456789",
        "!@#$%^&*()_+[]{}|;:,.<>?",
      ];

      let allChars = characters[0];

      if (this.includeUppercase) allChars += characters[1];
      if (this.includeNumbers) allChars += characters[2];
      if (this.includeSpecialChars) allChars += characters[3];

      let password = "";
      for (let i = 0; i < this.passwordLength; i++) {
        const randomIndex = Math.floor(Math.random() * allChars.length);
        password += allChars[randomIndex];
      }

      this.generatedPassword = password;
      this.calculatePasswordStrength();
    },
    calculatePasswordStrength() {
      // Check password strength
      if (this.passwordLength < 6) {
        this.passwordStrength = "Easy";
      } else if (
        this.includeUppercase &&
        this.includeLowercase &&
        this.includeNumbers &&
        this.includeSpecialChars
      ) {
        this.passwordStrength = "Super Strong";
      } else if (
        (this.includeUppercase && this.includeLowercase) ||
        this.includeNumbers ||
        this.includeSpecialChars
      ) {
        this.passwordStrength = "Strong";
      } else {
        this.passwordStrength = "Medium";
      }
    },
  },
};
</script>

<template>
  <div>
    <p>
      Password: {{ generatedPassword }}
      <button @click="copyToClipboard">Copy</button>
    </p>
  </div>
  <div id="app">
    <label for="passwordLength">Password Length: </label>
    <input
      type="range"
      id="passwordLength"
      v-model="passwordLength"
      min="4"
      max="20"
    />
    {{ passwordLength }}

    <div>
      <input type="checkbox" id="includeNumbers" v-model="includeNumbers" />
      <label for="includeNumbers">Include Numbers</label>
    </div>

    <div>
      <input type="checkbox" id="includeUppercase" v-model="includeUppercase" />
      <label for="includeUppercase">Include Uppercase Letters</label>
    </div>

    <div>
      <input type="checkbox" id="includeLowercase" v-model="includeLowercase" />
      <label for="includeLowercase">Include Lowercase Letters</label>
    </div>

    <div>
      <input
        type="checkbox"
        id="includeSpecialChars"
        v-model="includeSpecialChars"
      />
      <label for="includeSpecialChars">Include Special Characters</label>
    </div>
    <div>
      <p>Password Strength {{ passwordStrength }}</p>
    </div>
    <button @click="generatePassword">Generate Password</button>
  </div>
</template>

<style scoped></style>
