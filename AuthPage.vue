<script setup>
import AppAuthForm from '@/components/AppAuthForm.vue'
</script>
<template>
  <div class="page">
    <AppAuthForm />
  </div>
</template>

<style lang="css" scoped>
.page {
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  background-image: var(--gradient-bg), url('@/assets/img/pozadina1.jpg');
  background-repeat: no-repeat;
  background-position: center;
  background-size: cover;
}
.logo {
  height: 80px;
}
</style>
