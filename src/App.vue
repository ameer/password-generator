<template>
  <section id="main">
    <div class="container main-card">
      <h1 class="mb-10 text-left">Password Generator</h1>
      <div class="t-bg generated-pass relative mb-8 py-5 relative">
        <p>{{ generatedPassword }}</p>
        <div class="copy-btn" @click.stop="copyToClipboard(generatedPassword)">
          {{ copyBtnText }}
        </div>
      </div>
      <p class="text-left mb-2">Length: {{ length }}</p>
      <div class="t-bg mb-8 py-4 px-4">
        <div class="d-flex align-center">
          <span class="mr-4">{{ minLength }}</span>
          <input
            ref="lengthSlider"
            @change="drawSliderBg"
            @input="drawSliderBg"
            type="range"
            v-model="length"
            :min="minLength"
            :max="maxLength"
            data-color="#f00"
            class="slider flex-grow-1"
            id="length-slider"
          />
          <span class="ml-4">{{ maxLength }}</span>
        </div>
      </div>
      <p class="text-left mb-2">Settings:</p>
      <div class="t-bg mb-2 py-4 px-4">
        <div class="d-flex align">
          <p class="flex-grow-1 text-left align-self-center">
            Include Uppercase
          </p>
          <div class="custom-cb">
            <input
              type="checkbox"
              name="uppercase-cb"
              id="uppercase"
              v-model="uppercase"
            />
            <label for="uppercase" class="pointer"></label>
          </div>
        </div>
      </div>
      <div class="t-bg mb-2 py-4 px-4">
        <div class="d-flex align">
          <p class="flex-grow-1 text-left align-self-center">
            Include Lowercase
          </p>
          <div class="custom-cb">
            <input
              type="checkbox"
              name="lowercase-cb"
              id="lowercase"
              v-model="lowercase"
            />
            <label for="lowercase" class="pointer"></label>
          </div>
        </div>
      </div>
      <div class="t-bg mb-2 py-4 px-4">
        <div class="d-flex align">
          <p class="flex-grow-1 text-left align-self-center">Include Numbers</p>
          <div class="custom-cb">
            <input
              type="checkbox"
              name="numbers-cb"
              id="numbers"
              v-model="numbers"
            />
            <label for="numbers" class="pointer"></label>
          </div>
        </div>
      </div>
      <div class="t-bg mb-2 py-4 px-4">
        <div class="d-flex align">
          <p class="flex-grow-1 text-left align-self-center">Include Symbols</p>
          <div class="custom-cb">
            <input
              type="checkbox"
              name="symbols-cb"
              id="symbols"
              v-model="symbols"
            />
            <label for="symbols" class="pointer"></label>
          </div>
        </div>
      </div>
      <div>
        <button class="btn" @click="generatePassword">Generate</button>
      </div>
    </div>
  </section>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      copyBtnText: "Copy",
      generatedPassword: "Click Generate",
      lcArray: "abcdefghijklmnopqrstuvwxyz",
      symArray: "!@#$%^&*()_+=-{}|:?>.<,",
      noArray: "1234567890",
      length: 16,
      minLength: 4,
      maxLength: 32,
      uppercase: false,
      lowercase: true,
      numbers: false,
      symbols: true,
    };
  },
  computed: {
    ucArray() {
      return this.lcArray.toUpperCase();
    },
  },
  mounted() {
    this.drawSliderBg();
  },
  methods: {
    drawSliderBg() {
      const valBg =
        ((this.length - this.minLength) / (this.maxLength - this.minLength)) *
        100;
      const color = "#0827c8";
      this.$refs.lengthSlider.style.background = `linear-gradient(to right, ${color} 0%, ${color} ${valBg}%, #fff ${valBg}%, white 100%)`;
    },
    generatePassword() {
      let str = "";
      let pass = "";
      if (this.uppercase) {
        str += this.ucArray;
      }
      if (this.lowercase) {
        str += this.lcArray;
      }
      if (this.numbers) {
        str += this.noArray;
      }
      if (this.symbols) {
        str += this.symArray;
      }
      for (let index = 0; index < this.length; index++) {
        let rnd = Math.floor(Math.random() * str.length);
        pass += str.charAt(rnd);
      }
      this.generatedPassword = pass;
    },
    copyToClipboard(text) {
      const self = this;
      var textarea = document.createElement("textarea");
      textarea.textContent = text;
      textarea.style.position = "fixed"; // Prevent scrolling to bottom of page in Microsoft Edge.
      document.body.appendChild(textarea);
      textarea.select();
      try {
        document.execCommand("copy");
        self.copyBtnText = "Copied!";
        window.setTimeout(() => {
          self.copyBtnText = "Copy";
        }, 3000);
      } catch (ex) {
        console.warn("Copy to clipboard failed.", ex);
        return false;
      } finally {
        document.body.removeChild(textarea);
      }
    },
  },
};
</script>

<style>
</style>
