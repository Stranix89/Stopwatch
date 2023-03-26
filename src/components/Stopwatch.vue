<template>
  <div class="stopwatches">
    <div
      class="stopwatch"
      :class="{ running: stopwatch.isRunning }"
      v-for="(stopwatch, index) in stopwatches"
      :key="index"
    >
      <div class="stopwatch-time">{{ formatTime(stopwatch.timer) }}</div>
      <div class="stopwatch-controls">
        <button
          v-if="!stopwatch.isRunning"
          class="stopwatch-button"
          @click="startWatch(stopwatch)"
          :disabled="stopwatch.isRunning"
        >
          <svg
            class="stopwatch-svg"
            width="17"
            height="20"
            viewBox="0 0 17 20"
            fill="#9E9E9E"
          >
            <path d="M0 20V0L17 10L0 20Z" />
          </svg>
        </button>
        <button
          v-else
          class="stopwatch-button"
          @click="pauseWatch(stopwatch)"
          :disabled="!stopwatch.isRunning"
        >
          <svg
            class="stopwatch-svg"
            width="10"
            height="20"
            viewBox="0 0 10 20"
            fill="#9E9E9E"
          >
            <rect x="7" width="3" height="20" />
            <rect width="3" height="20" />
          </svg>
        </button>
        <button class="stopwatch-button" @click="resetWatch(stopwatch)">
          <svg
            class="stopwatch-svg"
            width="20"
            height="20"
            viewBox="0 0 20 20"
            fill="#9E9E9E"
          >
            <rect width="20" height="20" />
          </svg>
        </button>
      </div>
    </div>
    <div class="stopwatch-add">
      <button class="stopwatch-button" @click="addStopwatch">
        <svg
          class="stopwatch-svg"
          width="20"
          height="20"
          viewBox="0 0 20 20"
          fill="#9E9E9E"
        >
          <rect x="8.5" width="3" height="20" />
          <rect y="11.5" width="3" height="20" transform="rotate(-90 0 11.5)" />
        </svg>
      </button>
    </div>
  </div>
</template>
<script setup>
import { ref } from "vue";
const stopwatches = ref([{ timer: 0, isRunning: false, intervalId: null }]);

const startWatch = (stopwatch) => {
  stopwatch.isRunning = true;
  stopwatch.intervalId = setInterval(() => {
    stopwatch.timer += 1000;
  }, 1000);
};

const pauseWatch = (stopwatch) => {
  stopwatch.isRunning = false;
  clearInterval(stopwatch.intervalId);
};

const resetWatch = (stopwatch) => {
  stopwatch.isRunning = false;
  clearInterval(stopwatch.intervalId);
  stopwatch.timer = 0;
};

const addStopwatch = () => {
  stopwatches.value.push({ timer: 0, isRunning: false, intervalId: null });
};

const formatTime = (time) => {
  const hours = Math.floor(time / 3600000);
  const minutes = Math.floor((time % 3600000) / 60000);
  const seconds = Math.floor((time % 60000) / 1000);
  if (hours > 0) {
    return `${hours.toString().padStart(2, "0")}:${minutes
      .toString()
      .padStart(2, "0")}:${seconds.toString().padStart(2, "0")}`;
  }
  if (minutes > 0) {
    return `${minutes.toString().padStart(2, "0")}:${seconds
      .toString()
      .padStart(2, "0")}`;
  } else {
    return `${seconds.toString().padStart(2, "0")}`;
  }
};
</script>

<style>
.stopwatches {
  padding: 72px 50px;
  justify-content: center;
  display: grid;
  grid-template-columns: repeat(auto-fit, 225px);
  gap: 45px 50px;
}
.stopwatch {
  text-align: center;
  background-color: var(--item-bg-color);
  color: #b2aeae;
  height: 130px;
}
.stopwatch-time {
  padding: 20px 0;
  border-bottom: 1px solid var(--item-text-color);
}
.stopwatch-controls {
  padding: 20px 0;
}
.stopwatch-button {
  background-color: transparent;
  border: none;
  cursor: pointer;
  margin-right: 48px;
}
.stopwatch-button:last-child {
  margin-right: 0;
}
.stopwatch-add {
  background-color: var(--item-bg-color);
  padding: 20px;
  display: flex;
  align-items: center;
  justify-content: center;
  height: 130px;
  cursor: pointer;
}
.running {
  color: var(--active-color);
}
.running .stopwatch-svg {
  fill: var(--active-color);
}
</style>
