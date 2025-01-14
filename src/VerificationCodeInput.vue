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
import { ref, watch } from 'vue';

const props = defineProps<{
  length: number;
}>();

const digits = ref(Array(props.length).fill(''));

watch(() => props.length, (newLength) => {
  digits.value = Array(newLength).fill('');
});

/**
 * 處理輸入事件
 * @param {Event} event - 輸入事件
 * @param {number} index - 當前輸入框的索引
 */
const onInput = (event: Event, index: number) => {
  const input = event.target as HTMLInputElement;
  const value = input.value.replace(/\D/g, '');
  digits.value[index] = value;
  if (value && index < props.length - 1) {
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
  if (index < props.length - 1) {
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
  for (let i = 0; i < paste.length && index + i < props.length; i++) {
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

.verification-code-input .input-content {
  width: 512px;
  height: 60px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.verification-code-input input {
  color: inherit;
  font-family: inherit;
  border: 0;
  outline: 0;
  border-bottom: 1px solid #919191;
  height: 60px;
  width: 60px;
  font-size: 44px;
  text-align: center;
}

.verification-code-input input::-webkit-outer-spin-button,
.verification-code-input input::-webkit-inner-spin-button {
  appearance: none;
  margin: 0;
}
</style>
