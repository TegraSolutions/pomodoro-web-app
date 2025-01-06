<template>
    <div v-if="isOpen" class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center">
        <Card class="p-0 w-96">
            <CardHeader class="text-center">
                <CardTitle class="text-2xl">Congratulations</CardTitle>
                <CardDescription>You have completed your session</CardDescription>
            </CardHeader>
            <CardContent class="text-center">
                <p>Total Time Elapsed: {{ elapsedTime }}</p>
            </CardContent>
            <CardFooter class="flex justify-center px-6 pb-6">
                <Button variant="ghost" @click="closeModal">
                    Close
                </Button>
            </CardFooter>
        </Card>
    </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import { Card, CardHeader, CardTitle, CardDescription, CardContent, CardFooter } from './ui/card';
import { Button } from './ui/button';

export default defineComponent({
    components: {
        Card,
        CardHeader,
        CardTitle,
        CardDescription,
        CardContent,
        CardFooter,
        Button,
    },
    props: {
        isOpen: {
            type: Boolean,
            required: true,
        },
        elapsedTime: {
            type: String,
            required: true,
        },
    },
    emits: ["close"],
    methods: {
        closeModal() {
            this.$emit("close");
        },
    },
});

</script>