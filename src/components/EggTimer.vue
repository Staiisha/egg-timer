<script setup lang="ts">
import { ref, watch, onBeforeUnmount } from 'vue';

const props = defineProps<{ time: number | null }>();
const emit = defineEmits<{
  (e: 'timeUp'): void;
}>();

const remainingTime = ref(props.time ? props.time * 60 : 0);
const timerActive = ref(false);
const intervalId = ref<number | null>(null); // Для хранения ID интервала

const startTimer = () => {
  if (!props.time) return;

  // Очищаем предыдущий интервал, если он есть
  if (intervalId.value !== null) {
    clearInterval(intervalId.value);
  }

  remainingTime.value = props.time * 60;
  timerActive.value = true;

  intervalId.value = setInterval(() => {
    if (remainingTime.value > 0) {
      remainingTime.value--;
    } else {
      clearInterval(intervalId.value!);
      timerActive.value = false;
      emit('timeUp');
    }
  }, 1000);
};

// Очищаем интервал при изменении props.time
watch(() => props.time, (newTime) => {
  if (newTime) {
    if (intervalId.value !== null) {
      clearInterval(intervalId.value);
      timerActive.value = false;
    }
    remainingTime.value = newTime * 60;
  }
});

// Очищаем интервал при уничтожении компонента
onBeforeUnmount(() => {
  if (intervalId.value !== null) {
    clearInterval(intervalId.value);
  }
});
</script>

<template>
  <div class="egg-timer">
    <h2 v-if="time">Таймер: {{ Math.floor(remainingTime / 60) }}:{{ (remainingTime % 60).toString().padStart(2, '0') }}</h2>
    <button v-if="time && !timerActive" @click="startTimer">Старт</button>
  </div>
</template>

<style scoped>
.egg-timer {
  text-align: center;
}

button {
  margin-top: 10px;
  padding: 10px;
  border: none;
  border-radius: 5px;
  background: #90be6d;
  cursor: pointer;
  font-size: 16px;
}
</style>