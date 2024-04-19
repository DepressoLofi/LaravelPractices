<script setup>
import { ref } from 'vue';

const props = defineProps([
    'rooms',
    'currentRoom'
])
const selected = ref(props.currentRoom);

</script>

<template>
    <div class="grid grid-cols-2">
        <div class="font-bold text-xl">
            {{ selected.name }} Chat
        </div>
        <select v-model="selected" @change="$emit('roomchanged', selected)" class="place-self-end float-right w-96">
            <option v-for="(room, index) in rooms" :key="index" :value="room">{{ room.name }}</option>
        </select>

    </div>
</template>

