<template>
  <div class="content">
    <router-view />
  </div>
</template>
<script setup lang="ts"></script>
<style scoped>
@font-face {
  font-family: TITLE;
  src: url('./assets/youshe.woff2');
}
.content {
  max-width: 480px;
  margin: 0 auto;
}
</style>
