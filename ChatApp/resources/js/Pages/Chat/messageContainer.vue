<script setup>
import MessageItem from '@/Pages/Chat/messageItem.vue';

const props = defineProps(['messages']);

</script>

<template>
    <div class="h-[35rem] w-full p-2 flex flex-col-reverse overflow-y-auto chat-bg border-b-4 border-pink-500">
       
            <div v-for="(message, index) in messages" :key="index">
                <message-item :message="message"></message-item>
            </div>
       
    </div>
</template>

<style>
.chat-bg{
    background-image: url('@/../images/chatbg.png');
    background-size: cover;
}
</style>

