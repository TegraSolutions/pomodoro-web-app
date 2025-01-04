<template>
    <div class="flex justify-center gap-4">
      <button @click="startTimer" class="bg-green-500 text-white px-4 py-2 rounded">Start</button>
      <button @click="pauseTimer" class="bg-yellow-500 text-white px-4 py-2 rounded">Pause</button>
      <button @click="resetTimer" class="bg-red-500 text-white px-4 py-2 rounded">Reset</button>
    </div>
</template>

<script lang="ts">
import { defineComponent, PropType } from 'vue';

export default defineComponent({
    emits: ['start', 'pause', 'reset'],
    methods: {
        startTimer() {
            this.$emit('start');
        },
        pauseTimer() {
            this.$emit('pause');
        },
        resetTimer() {
            this.$emit('reset');
        }
    },
})

</script>

<style scoped>

</style>