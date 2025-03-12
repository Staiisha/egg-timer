<script setup lang="ts">
import { defineEmits } from 'vue';

const emit = defineEmits<{
  (e: 'selectEgg', time: number): void;
}>();

const eggs = [
  { name: 'Всмятку', time: 3 },
  { name: 'Мешочек', time: 5 },
  { name: 'Вкрутую', time: 8 }
];

const selectEgg = (time: number) => {
  emit('selectEgg', time);
};
</script>

<template>
  <div class="egg-selector">
    <h2>Выберите тип яйца:</h2>
    <button v-for="egg in eggs" :key="egg.name" @click="selectEgg(egg.time)">
      {{ egg.name }} ({{ egg.time }} мин)
    </button>
  </div>
</template>

<style scoped>
.egg-selector {
  text-align: center;
  margin-bottom: 20px;
}

button {
  margin: 5px;
  padding: 10px;
  border: none;
  border-radius: 5px;
  background: #f9c74f;
  cursor: pointer;
  font-size: 16px;
}
</style>
