<script setup>
const emit = defineEmits(['startGame']);
</script>

<template>
  <button
    class="controls"
    @click="emit('startGame')"
  >
    Начать
  </button>
</template>

<style lang="scss" scoped>
.controls {
  position: absolute;
  inset: 25%;

  font-size: 2rem;
  color: #fff;
  background-color: #343432;
  border-radius: 50%;

  display: flex;
  align-items: center;
  justify-content: center;
}
</style>
