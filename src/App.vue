<template>
  <div class="min-h-screen flex flex-col items-center justify-center bg-gray-100">
    <TimerDisplay :timeLeft="timeLeft" :isWorkTime="isWorkTime" />
    <TimerControls
      @start="startTimer"
      @pause="pauseTimer"
      @reset="resetTimer"
    />
    <SettingsModal
      :isOpen="settingsOpen"
      @close="toggleSettings"
      @save="updateSettings"
    />
    <button @click="toggleSettings" class="mt-6 bg-blue-500 text-white px-4 py-2 rounded">
      Settings
    </button>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';
import TimerDisplay from './components/TimerDisplay.vue';
import TimerControls from './components/TimerControls.vue';
import SettingsModal from './components/SettingsModal.vue';

export default defineComponent({
  components: { TimerDisplay, TimerControls, SettingsModal },
  setup() {
    const workDuration = ref(25 * 60); // Default 25 minutes
    const breakDuration = ref(5 * 60); // Default 5 minutes
    const timeLeft = ref(workDuration.value);
    const timer = ref<ReturnType<typeof setInterval> | null>(null);
    const settingsOpen = ref(false);
    const isWorkTime = ref(true);

    const startTimer = () => {
      if (!timer.value) {
        timer.value = setInterval(() => {
          if (timeLeft.value > 0) {
            timeLeft.value--;
          } else {
            switchMode();
          }
        }, 1000);
      }
    };

    const pauseTimer = () => {
      if (timer.value) {
        clearInterval(timer.value);
        timer.value = null;
      }
    };

    const resetTimer = () => {
      pauseTimer();
      timeLeft.value = isWorkTime.value ? workDuration.value : breakDuration.value;
    };

    const switchMode = () => {
      isWorkTime.value = !isWorkTime.value;
      timeLeft.value = isWorkTime.value ? workDuration.value : breakDuration.value;
    };

    const toggleSettings = () => {
      settingsOpen.value = !settingsOpen.value;
    };

    const updateSettings = (settings: { workDuration: number; breakDuration: number }) => {
      workDuration.value = settings.workDuration * 60;
      breakDuration.value = settings.breakDuration * 60;
      resetTimer();
      toggleSettings();
    };

    return {
      timeLeft,
      startTimer,
      pauseTimer,
      resetTimer,
      settingsOpen,
      toggleSettings,
      updateSettings,
      isWorkTime,
    };
  },
});
</script>