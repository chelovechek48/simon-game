<script setup>
import { ref, onMounted } from 'vue';

const getDeg = (index) => {
  const deg = index * 90;
  const isSecondRow = deg > 90;
  return isSecondRow ? (90 - deg) : deg;
};

const session = ref({
  colorsList: ['red', 'green', 'blue', 'gray'],
  comboList: [],
  activeColor: null,
});

const getRandomIndex = () => {
  const maxIndex = session.value.colorsList.length;
  return Math.floor(Math.random() * maxIndex);
};

const setCombo = () => {
  const prevCount = session.value.comboList.length;
  const minCount = 3;
  const count = (prevCount >= minCount) ? (prevCount + 1) : minCount;

  const newCombo = Array.from(
    { length: count },
    () => session.value.colorsList[getRandomIndex()],
  );

  session.value.comboList = newCombo;
};

const play = () => {
  const { comboList } = session.value;

  const minDuration = 3000;
  const minDelay = 500;

  const comboLength = comboList.length;
  const calculatedDelay = minDuration / comboLength;
  const delay = calculatedDelay < minDelay ? minDelay : calculatedDelay;

  comboList.forEach((color, index) => {
    setTimeout(() => {
      session.value.activeColor = color;
    }, delay * index);
  });

  setTimeout(() => {
    session.value.activeColor = null;
  }, delay * comboLength);
};

onMounted(() => {
  setCombo();
  play();
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
      <button
        :class="session.activeColor === color
          ? 'list__button list__button_active'
          : 'list__button'"
      />
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

    &_active,
    &:focus-visible {
      outline: none;
      border: 0.5rem solid #000;
    }
  }
}
</style>
