<template>
  <main>
    <div class="container" ref="container">
      <input
        v-for="(input, index) in otpLength"
        :key="index"
        ref="otpInput"
        v-model="otpArray[index]"
        @keydown="handleEnter(index, $event)"
        @input="handleInput(index, $event)"
        @paste="handlePaste(index, $event)"
        type="text"
        step="1"
        maxlength="1"
        class="input"
      />
    </div>
  </main>
</template>
<script>
export default {
  data() {
    return {
      joinedValue: "",
      otpArray: [],
      error: "",
    };
  },
  props: {
    otpLength: {
      type: Number,
      default: 6,
    },
  },
  methods: {
    handleEnter(index, event) {
      const keypressed = event.keyCode || event.which;
      if (
        (keypressed < 48 || keypressed > 57) &&
        keypressed !== 8 &&
        !event.ctrlKey
      ) {
        event.preventDefault();
      }
      if (keypressed === 8 || keypressed === 46) {
        event.preventDefault();
        if (!this.otpArray[index] && index > 0) {
          this.otpArray[index - 1] = "";
          this.$refs.otpInput[index - 1].value = "";
          this.$refs.otpInput[Math.max(0, index - 1)].focus();
        } else {
          this.otpArray[index] = "";
          this.$refs.otpInput[index].value = "";
        }
      }
    },
    handleInput(index, event) {
      if (index < this.otpLength - 1) {
        this.$refs.otpInput[index + 1].focus();
      }
      if (index === this.otpLength - 1) {
        this.$refs.otpInput[index].blur();
      }
      
      if (event.inputType === "insertFromPaste") {
        this.handlePasteEvent(index)
      }
      this.joinedValue = this.otpArray.join("");
          if (this.joinedValue.length === this.otpLength) {
            this.$emit("otp-complete", this.joinedValue);
          }
    },
    handlePasteEvent(index){
      navigator.clipboard.readText().then((pastedText) => {
          const shavedText = pastedText.replace(/[^0-9]/g, '')
          for (let i = 0; i < shavedText.length && index + i < this.otpLength; i++) {
            this.otpArray[index + i] = shavedText.charAt(i);
            if (index + i + 1 < this.otpLength) {
              this.$refs.otpInput[index + i + 1].dispatchEvent(
                new Event("input")
              );
            }
          }
        });
    },
    handlePaste(index, event) {
      this.handlePasteEvent(index)
      this.$emit("paste", event);
    },
  },
};
</script>
<style>
main {
  display: flex;
  flex-direction: column;
  justify-content: center;
  height: 80vh;
}
.container {
  display: flex;
  gap: 1rem;
  margin: 1rem auto;
  width: 80%;
  justify-content: center;
}

.input {
  outline: none;
  width: 2rem;
  display: flex;
  justify-content: center;
  padding: 0px;
  margin-right: 1rem;
  color: inherit;
  border: 1px solid #222222;
  border-radius: 8px;
  padding: 0.85rem;
  padding-right: 0;
  font-family: inherit;
  font-size: 1.5rem;
  font-weight: 700;
  line-height: 20px;
  appearance: none;
  letter-spacing: 18px;
}
.input:focus {
  border: 4px solid #222222;
}
</style>
