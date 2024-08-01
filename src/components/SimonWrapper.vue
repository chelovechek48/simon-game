<script setup>
import { ref } from 'vue';
import SimonList from '@components/SimonList.vue';
import SimonControls from '@components/SimonControls.vue';

const session = ref({
  colorsList: ['#34c940', '#f22412', '#f7ea29', '#0189de'],
  comboList: {
    valid: [],
    player: [],
  },
  activeColor: null,
  disabled: true,
});

const getRandomIndex = () => {
  const maxIndex = session.value.colorsList.length;
  return Math.floor(Math.random() * maxIndex);
};

const extendCombo = () => {
  const newComboItem = session.value.colorsList[getRandomIndex()];
  session.value.comboList.valid.push(newComboItem);
  session.value.comboList.player = [];
};

const showCombo = () => {
  const comboList = session.value.comboList.valid;

  const delay = 1000;
  const interval = delay / 3;

  comboList.forEach((color, index) => {
    setTimeout(() => {
      session.value.activeColor = null;
      setTimeout(() => {
        session.value.activeColor = color;
      }, interval);
    }, delay * index);
  });

  session.value.disabled = true;
  const duration = delay * comboList.length;
  setTimeout(() => {
    session.value.activeColor = null;
    setTimeout(() => {
      session.value.disabled = false;
    }, interval);
  }, duration);
};

const launchCombo = () => {
  extendCombo();
  showCombo();
};

const finishGame = () => {
  session.value.disabled = true;
  session.value.comboList.valid = [];
};

const checkOption = (option) => {
  const { valid, player } = session.value.comboList;
  player.push(option);

  const index = player.length - 1;
  const lengthIsEqual = player.length === valid.length;
  const optionIsEqual = player[index] === valid[index];
  if (optionIsEqual) {
    if (lengthIsEqual) {
      launchCombo();
    }
  } else {
    finishGame();
  }
};
</script>

<template>
  <section class="simon">
    <SimonList
      :session="session"
      @selectSector="checkOption($event)"
    />
    <SimonControls
      :session="session"
      @startGame="launchCombo()"
      @finishGame="finishGame()"
    />
  </section>
</template>

<style lang="scss" scoped>
.simon {
  width: 30rem;
  position: relative;
}
</style>
