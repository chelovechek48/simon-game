<script setup>
import SimonWrapper from '@components/SimonWrapper.vue';
</script>

<template>
  <main class="page">
    <SimonWrapper />
  </main>
</template>

<style lang="scss">
@import '@styles';
</style>

<style lang="scss" scoped>
.page {
  min-height: 100dvh;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 0.75rem;

  background-color: #343432;
}
</style>
