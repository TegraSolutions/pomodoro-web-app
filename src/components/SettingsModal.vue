<template>
    <div v-if="isOpen" class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center">
    <div class="bg-white p-6 rounded shadow-lg">
      <h2 class="text-xl font-bold mb-4">Settings</h2>
      <div>
        <label class="block mb-2">Work Duration (minutes):</label>
        <input v-model="workDuration" type="number" class="border rounded p-2 w-full">
      </div>
      <div class="mt-4">
        <label class="block mb-2">Break Duration (minutes):</label>
        <input v-model="breakDuration" type="number" class="border rounded p-2 w-full">
      </div>
      <div class="flex justify-end mt-6 gap-4">
        <button @click="saveSettings" class="bg-blue-500 text-white px-4 py-2 rounded">Save</button>
        <button @click="closeModal" class="bg-gray-500 text-white px-4 py-2 rounded">Cancel</button>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent }  from 'vue';

export default defineComponent({
    props: {
        isOpen: Boolean,
    },
    emits: ['close', 'save'],
    data() {
        return {
            workDuration: 25,
            breakDuration: 5,
        };
    },
    methods: {
        closeModal() {
            this.$emit('close');
        },
        saveSettings() {
            this.$emit('save', {
                workDuration: this.workDuration,
                breakDuration: this.breakDuration,
            });
        }
    }
});
</script>

<style scoped>

</style>