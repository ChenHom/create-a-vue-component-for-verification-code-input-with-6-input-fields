<script setup lang="ts">
import { onMounted } from 'vue';
import VerificationCodeInput from './VerificationCodeInput.vue';

onMounted(() => {
  const firstInput = document.querySelector('.verification-code-input input') as HTMLInputElement;
  if (firstInput) {
    firstInput.focus();
  }
});
</script>

<template>
  <div class="container">
    <VerificationCodeInput />
  </div>
</template>

<style scoped>
.container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}
</style>
