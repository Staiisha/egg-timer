<script setup lang="ts">
import { ref } from 'vue';
import EggSelector from './components/EggSelector.vue';
import EggTimer from './components/EggTimer.vue';
import EggResult from './components/EggResult.vue';

const selectedTime = ref<number | null>(null);
const eggReady = ref(false);

const handleSelectEgg = (time: number) => {
  selectedTime.value = time;
  eggReady.value = false;
};

const handleTimeUp = () => {
  eggReady.value = true;
};
</script>

<template>
  <div class="app">
    <h1> Таймер для варки яиц</h1>
    <EggSelector @selectEgg="handleSelectEgg" />
    <EggTimer v-if="selectedTime !== null" :time="selectedTime" @timeUp="handleTimeUp" />
    <EggResult v-if="eggReady" />
  </div>
</template>

<style scoped>
.app {
  text-align: center;
  font-family: Arial, sans-serif;
  padding: 20px;
}
</style>
