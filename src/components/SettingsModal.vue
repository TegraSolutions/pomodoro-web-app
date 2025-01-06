<template>
    <div v-if="isOpen" class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center">
        <Card class="p-0 w-96">
            <CardHeader>
                <CardTitle class="text-2xl">Settings</CardTitle>
                <CardDescription>Change the duration of your work and break sessions.</CardDescription>
            </CardHeader>
            <CardContent>
                <div class="grid items-center w-full gap-4">
                    <div class="flex flex-col space-y-1.5">
                        <Label for="workDuration">Work Duration (minutes):</Label>
                        <Input id="workDuration" placeholder="Name of your project" v-model="workDuration" type="number" />
                    </div>
                    <div class="flex flex-col space-y-1.5">
                        <Label for="breakDuration">Break Duration (minutes):</Label>
                        <Input id="breakDuration" placeholder="Name of your project" v-model="breakDuration" type="number" />
                    </div>
                    <div class="flex flex-col space-y-1.5">
                        <Label for="maxCycles">Max Cycles:</Label>
                        <Input id="maxCycles" placeholder="Name of your project" v-model="maxCycles" type="number" />
                    </div>
                </div>
            </CardContent>
            <CardFooter class="flex justify-between px-6 pb-6">
                <Button variant="destructive" @click="closeModal">
                    Cancel
                </Button>
                <Button @click="saveSettings" variant="ghost">Save</Button>
            </CardFooter>
        </Card>
    </div>    
</template>

<script lang="ts">
import { defineComponent }  from 'vue';
import { Card,CardHeader, CardTitle, CardDescription, CardContent, CardFooter } from './ui/card';
import { Label } from './ui/label';
import { Input } from './ui/input';
import { Button } from './ui/button';

export default defineComponent({
    components: {
        Card,
        CardHeader,
        CardTitle,
        CardDescription,
        CardContent,
        CardFooter,
        Label,
        Input,
        Button,
    },
    props: {
        isOpen: Boolean,
    },
    emits: ['close', 'save'],
    data() {
        return {
            workDuration: 25,
            breakDuration: 5,
            maxCycles: 4,
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
                maxCycles: this.maxCycles,
            });
        }
    }
});
</script>

<style scoped>

</style>