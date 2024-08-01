<script setup>
const emit = defineEmits(['startGame', 'finishGame']);
const props = defineProps({
  session: {
    type: Object,
    required: true,
  },
});

const gameIsStarted = () => props.session.comboList.valid.length;
const score = () => props.session.comboList.valid.length || 'Начать';

const progress = () => {
  const player = props.session.comboList.player.length;
  const valid = props.session.comboList.valid.length;
  return `${player} / ${valid}`;
};
</script>

<template>
  <button
    v-if="session.disabled"
    :disabled="gameIsStarted()"
    class="controls"
    @click="emit('startGame')"
  >
    {{ score() }}
  </button>
  <button
    v-else
    class="controls"
    @click="emit('finishGame')"
  >
    {{ progress() }}
  </button>
</template>

<style lang="scss" scoped>
.controls {
  position: absolute;
  inset: 25%;

  font-size: clamp(1rem, 10vw, 3rem);
  color: #fff;
  background-color: #343432;
  border-radius: 50%;

  display: flex;
  align-items: center;
  justify-content: center;
}
</style>
