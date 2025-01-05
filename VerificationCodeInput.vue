<template>
  <div class="verification-code-input">
    <input
      v-for="(digit, index) in digits"
      :key="index"
      type="text"
      maxlength="1"
      v-model="digits[index]"
      @input="onInput($event, index)"
      @keydown.backspace="onBackspace($event, index)"
      @keydown.left="onArrowLeft($event, index)"
      @keydown.right="onArrowRight($event, index)"
      @paste="onPaste($event, index)"
      @wheel="onWheel($event, index)"
      @focus="onFocus($event)"
    />
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';

export default defineComponent({
  name: 'VerificationCodeInput',
  setup() {
    const digits = ref(Array(6).fill(''));

    const onInput = (event: Event, index: number) => {
      const input = event.target as HTMLInputElement;
      const value = input.value.replace(/\D/g, '');
      digits.value[index] = value;
      if (value && index < 5) {
        ((event.target as HTMLInputElement).nextElementSibling as HTMLInputElement)?.focus();
      }
      input.setSelectionRange(1, 1);
    };

    const onBackspace = (event: KeyboardEvent, index: number) => {
      if (index > 0 && !digits.value[index]) {
        ((event.target as HTMLInputElement).previousElementSibling as HTMLInputElement)?.focus();
      } else {
        digits.value[index] = '';
      }
    };

    const onArrowLeft = (event: KeyboardEvent, index: number) => {
      if (index > 0) {
        ((event.target as HTMLInputElement).previousElementSibling as HTMLInputElement)?.focus();
      }
    };

    const onArrowRight = (event: KeyboardEvent, index: number) => {
      if (index < 5) {
        ((event.target as HTMLInputElement).nextElementSibling as HTMLInputElement)?.focus();
      }
    };

    const onPaste = (event: ClipboardEvent, index: number) => {
      const paste = event.clipboardData?.getData('text').replace(/\D/g, '') || '';
      for (let i = 0; i < paste.length && index + i < 6; i++) {
        digits.value[index + i] = paste[i];
      }
      event.preventDefault();
    };

    const onWheel = (event: WheelEvent, index: number) => {
      const delta = Math.sign(event.deltaY);
      const currentDigit = parseInt(digits.value[index] || '0', 10);
      const newDigit = (currentDigit + delta + 10) % 10;
      digits.value[index] = newDigit.toString();
      event.preventDefault();
    };

    const onFocus = (event: FocusEvent) => {
      (event.target as HTMLInputElement).select();
    };

    return {
      digits,
      onInput,
      onBackspace,
      onArrowLeft,
      onArrowRight,
      onPaste,
      onWheel,
      onFocus,
    };
  },
});
</script>

<style scoped>
.verification-code-input {
  display: flex;
  gap: 8px;
}

.verification-code-input input {
  width: 40px;
  height: 40px;
  text-align: center;
  font-size: 24px;
}
</style>
