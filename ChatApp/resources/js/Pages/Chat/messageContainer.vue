<script setup>
import MessageItem from '@/Pages/Chat/messageItem.vue';

const props = defineProps(['messages']);

</script>

<template>
    <div class="h-96 w-full">
       <div class="h-full p-2 flex flex-col-reverse ">
            <div v-for="(message, index) in messages" :key="index">
                <message-item :message="message"></message-item>
            </div>
       </div>
    </div>
</template>

