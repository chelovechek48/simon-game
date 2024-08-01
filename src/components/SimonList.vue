<script setup>
import { ref, onMounted } from 'vue';

const getDeg = (index) => {
  const deg = index * 90;
  const isSecondRow = deg > 90;
  return isSecondRow ? (90 - deg) : deg;
};

const session = ref({
  colorsList: ['#34c940', '#f22412', '#f7ea29', '#0189de'],
  comboList: [],
  activeColor: null,
  disabled: true,
});
const itemIsNotDisabled = (color) => {
  const sessionIsNotDisabled = !session.value.disabled;
  const isActiveColor = session.value.activeColor === color;
  return sessionIsNotDisabled || isActiveColor;
};

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

const showCombo = () => {
  const { comboList } = session.value;

  const delay = 1500;
  const interval = delay / 3;

  comboList.forEach((color, index) => {
    setTimeout(() => {
      session.value.activeColor = null;
      setTimeout(() => {
        session.value.activeColor = color;
      }, interval);
    }, delay * index);
  });

  const duration = delay * comboList.length + interval;
  session.value.disabled = true;
  setTimeout(() => {
    session.value.disabled = false;
    session.value.activeColor = null;
  }, duration);
};

onMounted(() => {
  setCombo();
  setTimeout(() => {
    showCombo();
  }, 200);
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
        :disabled="session.disabled"
        :class="itemIsNotDisabled(color)
          ? 'list__button'
          : 'list__button list__button_hidden'"
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

    transition: opacity 125ms ease;
    &_hidden {
      opacity: 0.25;
    }
  }
}
</style>
