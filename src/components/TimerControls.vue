<template>
    <div class="flex items-center gap-4">
      <Button @click="toggleTimer" variant="default" size="lg" class="text-2xl font-medium py-6">
        {{ isRunning ? 'pause' : 'start' }}
      </Button>
      <button @click="handleReset" class="relative">
        <MdRefresh :class="{'rotate-animation': isRotating}" class="w-12 h-12 text-background font-semibold" />
      </button>
    </div>
  </template>
  
  <script lang="ts">
  import { defineComponent, ref, computed } from 'vue';
  import { Button } from '@/components/ui/button'
  import { MdRefresh } from '@kalimahapps/vue-icons';
  export default defineComponent({
    components: {
      Button,
      MdRefresh
    },
    props: {
      isRunning: {
        type: Boolean,
        required: true
      }
    },
    emits: ['toggle', 'reset'],
    setup(props, { emit }) {
    const isRotating = ref(false);

    const buttonVariant = computed(() => (props.isRunning ? 'outline' : 'default'));

    const toggleTimer = () => {
      emit('toggle');
    };

    const handleReset = () => {
      isRotating.value = true;
      setTimeout(() => {
        isRotating.value = false;
      }, 1000); // Duration of the animation
      emit('reset');
    };

    return {
      toggleTimer,
      handleReset,
      isRotating,
      buttonVariant
    };
  }
  });
  </script>
  
  <style scoped>
  @keyframes rotate360 {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}

.rotate-animation {
  animation: rotate360 0.8s ease-in-out;
}
  </style>
