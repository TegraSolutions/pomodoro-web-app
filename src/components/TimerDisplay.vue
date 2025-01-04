<template>
    <div class="text-center">
      <h1 class="text-4xl font-bold">{{ formattedTime }}</h1>
      <p class="text-lg">{{ isWorkTime ? 'Work Time' : 'Break Time' }}</p>
    </div>
  </template>
  
  <script lang="ts">
  import { defineComponent } from 'vue';
  
  export default defineComponent({
    props: {
      timeLeft: {
        type: Number,
        required: true
      },
      isWorkTime: {
        type: Boolean,
        required: true
      }
    },
    computed: {
      formattedTime(): string {
        const minutes = Math.floor(this.timeLeft / 60);
        const seconds = this.timeLeft % 60;
        return `${minutes}:${seconds.toString().padStart(2, '0')}`;
      }
    }
  });
  </script>
  
  <style scoped>
  
  </style>