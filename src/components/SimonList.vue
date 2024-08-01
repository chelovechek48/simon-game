<script setup>
const props = defineProps({
  session: {
    type: Object,
    required: true,
  },
});

const getDeg = (index) => {
  const deg = index * 90;
  const isSecondRow = deg > 90;
  return isSecondRow ? (90 - deg) : deg;
};

const itemIsNotDisabled = (color) => {
  const sessionIsNotDisabled = !props.session.disabled;
  const isActiveColor = props.session.activeColor === color;
  return sessionIsNotDisabled || isActiveColor;
};
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
