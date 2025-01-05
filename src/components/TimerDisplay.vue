<template>
  <div class="text-center flex flex-col items-center gap-8">
    <div class="flex items-center gap-4">
      <Button :variant="workButtonVariant" size="lg" class="text-lg font-medium cursor-default">Work</Button>
      <Button :variant="breakButtonVariant" size="lg" class="text-lg font-medium cursor-default">Break</Button>
    </div>
    <h1 class="scroll-m-20 text-8xl font-semibold tracking-tight lg:text-9xl text-background">
      {{ formattedTime }}
    </h1>
  </div>
</template>
  
  <script lang="ts">
  import { defineComponent, computed } from 'vue';
  import { Button } from './ui/button';
  export default defineComponent({
    components: {
      Button
    },
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
    setup(props) {
    const workButtonVariant = computed(() => (props.isWorkTime ? 'secondary' : 'outline'));
    const breakButtonVariant = computed(() => (props.isWorkTime ? 'outline' : 'default'));

    const formattedTime = computed(() => {
      const minutes = Math.floor(props.timeLeft / 60);
      const seconds = props.timeLeft % 60;
      return `${minutes}:${seconds < 10 ? '0' : ''}${seconds}`;
    });

    return {
      workButtonVariant,
      breakButtonVariant,
      formattedTime
    };
  }
  });
  </script>
  
  <style scoped>
  
  </style>