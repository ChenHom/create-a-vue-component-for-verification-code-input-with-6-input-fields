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

<script setup lang="ts">
import { ref } from 'vue';

const digits = ref(Array(6).fill(''));

/**
 * 處理輸入事件
 * @param {Event} event - 輸入事件
 * @param {number} index - 當前輸入框的索引
 */
const onInput = (event: Event, index: number) => {
  const input = event.target as HTMLInputElement;
  const value = input.value.replace(/\D/g, '');
  digits.value[index] = value;
  if (value && index < 5) {
    ((event.target as HTMLInputElement).nextElementSibling as HTMLInputElement)?.focus();
  }
  input.setSelectionRange(1, 1);
};

/**
 * 處理退格鍵事件
 * @param {KeyboardEvent} event - 鍵盤事件
 * @param {number} index - 當前輸入框的索引
 */
const onBackspace = (event: KeyboardEvent, index: number) => {
  if (index > 0 && !digits.value[index]) {
    ((event.target as HTMLInputElement).previousElementSibling as HTMLInputElement)?.focus();
  } else {
    digits.value[index] = '';
  }
};

/**
 * 處理左箭頭鍵事件
 * @param {KeyboardEvent} event - 鍵盤事件
 * @param {number} index - 當前輸入框的索引
 */
const onArrowLeft = (event: KeyboardEvent, index: number) => {
  if (index > 0) {
    ((event.target as HTMLInputElement).previousElementSibling as HTMLInputElement)?.focus();
  }
};

/**
 * 處理右箭頭鍵事件
 * @param {KeyboardEvent} event - 鍵盤事件
 * @param {number} index - 當前輸入框的索引
 */
const onArrowRight = (event: KeyboardEvent, index: number) => {
  if (index < 5) {
    ((event.target as HTMLInputElement).nextElementSibling as HTMLInputElement)?.focus();
  }
};

/**
 * 處理貼上事件
 * @param {ClipboardEvent} event - 剪貼板事件
 * @param {number} index - 當前輸入框的索引
 */
const onPaste = (event: ClipboardEvent, index: number) => {
  const paste = event.clipboardData?.getData('text').replace(/\D/g, '') || '';
  for (let i = 0; i < paste.length && index + i < 6; i++) {
    digits.value[index + i] = paste[i];
  }
  event.preventDefault();
};

/**
 * 處理滾輪事件
 * @param {WheelEvent} event - 滾輪事件
 * @param {number} index - 當前輸入框的索引
 */
const onWheel = (event: WheelEvent, index: number) => {
  const delta = Math.sign(event.deltaY);
  const currentDigit = parseInt(digits.value[index] || '0', 10);
  const newDigit = (currentDigit + delta + 10) % 10;
  digits.value[index] = newDigit.toString();
  event.preventDefault();
};

/**
 * 處理焦點事件
 * @param {FocusEvent} event - 焦點事件
 */
const onFocus = (event: FocusEvent) => {
  (event.target as HTMLInputElement).select();
};
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
