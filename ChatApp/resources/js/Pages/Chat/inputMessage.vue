<script setup>
import { ref, defineEmits } from 'vue';

const emit = defineEmits(['message-sent']);
const props = defineProps(['room']);

const message = ref("");


const sendMessage = () => {
    
    if (message.value == ''){
        return;
    }

    axios.post(`/chat/room/${props.room.id}/message`, {
        message: message.value
    }).then(response => {
            message.value = '';
            emit('message-sent')
    }).catch(error => {
        console.log(error);
    })
}


</script>

<template>
    <div class=" h-10 m-1 z-1">

        <div class="grid grid-cols-6">
            <input type="text" v-model="message" 
            @keyup.enter="sendMessage()"
            placeholder="Say Something..."
            class="col-span-5 e p-1 border-transparent focus:border-transparent focus:ring-0" style="outline: none;"/>
            <button 
               @click="sendMessage()"
               class="place-self-center bg-yellow-500 hover:bg-yellow-600 p-1 mt-1 rounded text-white w-full">Send</button>
               
        </div>
        
    </div>
</template>

