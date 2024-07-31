<script setup>
import SimonList from '@components/SimonList.vue';
import SimonControls from '@components/SimonControls.vue';
</script>

<template>
  <section class="simon">
    <SimonList />
    <SimonControls />
  </section>
</template>

<style lang="scss" scoped>
.simon {
  width: 30rem;
  position: relative;
}
</style>
