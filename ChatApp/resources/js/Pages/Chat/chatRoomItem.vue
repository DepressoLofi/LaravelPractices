<script setup>
const props = defineProps(['chatRoom', 'currentRoom']);

</script>

<template>
    <div @click="$emit('roomchanged', chatRoom)" :class="{
            'py-4 px-4 text-lg border-b hover:bg-green-100 cursor-pointer': chatRoom !== currentRoom,
            'py-4 px-4 text-lg border-b bg-green-300 cursor-pointer': chatRoom === currentRoom
        }">
        {{ chatRoom.name }}
    </div>


</template>