<template>
    <div class="flex justify-center gap-4">
      <Button @click="toggleTimer" :class="isRunning ? 'bg-yellow-500' : 'bg-green-500'" class="text-white px-4 py-2 rounded">
        {{ isRunning ? 'Pause' : 'Start' }}
      </Button>
      <button @click="resetTimer" class="bg-red-500 text-white px-4 py-2 rounded">Reset</button>
    </div>
  </template>
  
  <script lang="ts">
  import { defineComponent } from 'vue';
  import { Button } from './ui/button';
  
  export default defineComponent({
    props: {
      isRunning: {
        type: Boolean,
        required: true
      }
    },
    emits: ['toggle', 'reset'],
    methods: {
      toggleTimer() {
        this.$emit('toggle');
      },
      resetTimer() {
        this.$emit('reset');
      }
    }
  });
  </script>
  
  <style scoped>
  </style>