<template>
  <div
    class="min-h-screen flex flex-col items-center justify-center bg-cover bg-center gap-8"
    style="background-image: url('https://studywithme.io/aesthetic-pomodoro-timer/9f1e88521119e2e349d8.jpg');"
  >
    <TimerDisplay :timeLeft="timeLeft" :isWorkTime="isWorkTime" />
    <div class="flex gap-4">
      <TimerControls
        :isRunning="isRunning"
        @toggle="toggleTimer"
        @reset="resetTimer"
      />
      <button
        @click="toggleSettings"
      >
        <FlFilledSettings class="w-12 h-12 text-background" />
      </button>
    </div>
    <SettingsModal
      :isOpen="settingsOpen"
      @close="toggleSettings"
      @save="updateSettings"
    />
    <div class="text-center mt-4">
      <p class="text-lg font-semibold text-background">Total Time Elapsed: {{ elapsedTime }}</p>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, computed, watch } from "vue";
import TimerDisplay from "./components/TimerDisplay.vue";
import TimerControls from "./components/TimerControls.vue";
import SettingsModal from "./components/SettingsModal.vue";
import { FlFilledSettings } from '@kalimahapps/vue-icons';

export default defineComponent({
  components: { TimerDisplay, TimerControls, SettingsModal, FlFilledSettings },
  setup() {
    const workDuration = ref(25 * 60); // Default 25 minutes
    const breakDuration = ref(5 * 60); // Default 5 minutes
    const timeLeft = ref(workDuration.value);
    const timer = ref<ReturnType<typeof setInterval> | null>(null);
    const settingsOpen = ref(false);
    const isWorkTime = ref(true);
    const isRunning = ref(false);
    const tickSound = new Audio('/sounds/tick.mp3');
    const restBell = new Audio('/sounds/rest-bell.mp3');
    const workBell = new Audio('/sounds/work-bell.mp3');
    const endBell = new Audio('/sounds/end-bell.mp3');
    const cycleCount = ref(0);
    const maxCycles = ref(5);
    const totalTimeElapsed = ref(0); // Counter for total time elapsed

    const startTimer = () => {
      if (!timer.value) {
        timer.value = setInterval(() => {
          if (timeLeft.value > 0) {
            timeLeft.value--;
            totalTimeElapsed.value++; // Increment total time elapsed
          } else {
            switchMode();
          }
        }, 1000);
        isRunning.value = true;
      }
    };

    const pauseTimer = () => {
      if (timer.value) {
        clearInterval(timer.value);
        timer.value = null;
        isRunning.value = false;
      }
    };

    const resetTimer = () => {
      pauseTimer();
      timeLeft.value = isWorkTime.value
        ? workDuration.value
        : breakDuration.value;
      cycleCount.value = 0;
      totalTimeElapsed.value = 0; // Reset total time elapsed
    };

    const switchMode = () => {
      if (cycleCount.value < maxCycles.value) {
        isWorkTime.value = !isWorkTime.value;
        timeLeft.value = isWorkTime.value
          ? workDuration.value
          : breakDuration.value;
        if (isWorkTime.value) {
          workBell.play();
        } else {
          restBell.play();
        }
        if (!isWorkTime.value) {
          cycleCount.value++;
        }
      } else {
        pauseTimer();
        endBell.play();
        resetTimer(); // Reset the timer to initial values
      }
    };

    const toggleSettings = () => {
      settingsOpen.value = !settingsOpen.value;
    };

    const updateSettings = (settings: {
      workDuration: number;
      breakDuration: number;
      maxCycles: number;
    }) => {
      workDuration.value = settings.workDuration * 60;
      breakDuration.value = settings.breakDuration * 60;
      maxCycles.value = settings.maxCycles;
      resetTimer();
      toggleSettings();
    };

    const toggleTimer = () => {
      if (isRunning.value) {
        pauseTimer();
      } else {
        startTimer();
      }
    };

    const elapsedTime = computed(() => {
      const hours = Math.floor(totalTimeElapsed.value / 3600);
      const minutes = Math.floor((totalTimeElapsed.value % 3600) / 60);
      return `${hours}h ${minutes}m`;
    });

    watch(timeLeft, (newTime) => {
      if (newTime <= 5 && newTime > 0) {
        tickSound.play();
      }
    });

    return {
      timeLeft,
      startTimer,
      pauseTimer,
      resetTimer,
      settingsOpen,
      toggleSettings,
      updateSettings,
      isWorkTime,
      isRunning,
      toggleTimer,
      cycleCount,
      maxCycles,
      elapsedTime,
    };
  },
});
</script>