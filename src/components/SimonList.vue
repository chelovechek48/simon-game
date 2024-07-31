<script setup>
import { ref } from 'vue';

const getDeg = (index) => {
  const deg = index * 90;
  const isSecondRow = deg > 90;
  return isSecondRow ? (90 - deg) : deg;
};

const session = ref({
  colorsList: ['red', 'green', 'blue', 'gray'],
});

</script>

<template>
  <ul class="list">
    <li
      class="list__item"
      v-for="(color, index) in session.colorsList"
      :key="color"
      :style="`
        --color: ${color};
        --rotate-deg: ${getDeg(index)}deg;
      `"
    >
      <button class="list__button" />
    </li>
  </ul>
</template>

<style lang="scss" scoped>
.list {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 0.5rem;

  &__item {
    display: flex;
  }

  &__button {
    width: 100%;
    aspect-ratio: 1;
    background-color: var(--color);
    border-top-left-radius: 100%;
    rotate: var(--rotate-deg);

    &:focus-visible {
      outline: none;
      border: 0.5rem solid #000;
    }
  }
}
</style>
