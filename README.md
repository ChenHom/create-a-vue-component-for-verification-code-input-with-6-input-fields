# 6 輸入框驗證碼組件

這是一個 Vue 3 組件，提供 6 個輸入框來輸入驗證碼。每個輸入框只允許輸入一個數字，並且支援多種輸入方式。

## 功能

- 限制輸入數字
- 正常輸入
- 遊標選取一個數字，滾輪可以微調數字大小，限制 0-9
- 用左右鍵切換輸入框時自動全選與點選輸入框也會自動全選
- backspace 刪除
- paste 任意位置貼上輸入
- 自動覆蓋遊標後輸入的字元
- 封裝成 Vue 單一檔案元件，方便任意呼叫

## 技術規格

1. 使用 Vue 3 Composition API
2. 支援 TypeScript
3. 支援鍵盤、滑鼠、觸控等多種輸入方式
4. 無外部依賴

## 安裝與使用

1. 克隆此專案：
    ```bash
    git clone https://github.com/your-repo/verification-code-input.git
    cd verification-code-input
    ```

2. 安裝依賴：
    ```bash
    npm install
    ```

3. 啟動開發伺服器：
    ```bash
    npm run dev
    ```

4. 在瀏覽器中打開 `http://localhost:3000` 查看效果。

## 組件使用

在你的 Vue 組件中引入並使用 `VerificationCodeInput`：

```vue
<script setup lang="ts">
import VerificationCodeInput from './VerificationCodeInput.vue';
</script>

<template>
  <VerificationCodeInput />
</template>
```
