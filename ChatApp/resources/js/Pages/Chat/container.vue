<script setup>
import AppLayout from '@/Layouts/AppLayout.vue';
import MessageContainer from '@/Pages/Chat/messageContainer.vue';
import InputMessage from "@/Pages/Chat/inputMessage.vue";
import chatRoomSelection from '@/Pages/Chat/chatRoomSelection.vue';
import { getCurrentInstance, ref, watch, reactive } from 'vue';

//Data Property
const chatRooms = ref([]);
const currentRoom = ref(null);
const messages = ref([]);

watch(currentRoom, (val, oldVal)=> {
    if(oldVal){
        disconnect(oldVal);
    }
    connect();
})

//Methods
const connect = () => {
    if(currentRoom.value.id){
        getMessages();
        window.Echo.private("chat." + currentRoom.value.id)
        .listen('.message.new', e=>{
            console.log("new message income");
            getMessages();
        })
    }
}

const disconnect = (room) => {
    window.Echo.leave("chat."+ room.id);
}

const getRooms = () => {
    axios.get('chat/rooms')
    .then(response => {
        chatRooms.value = response.data;
        setRoom(response.data[0]);

    })
    .catch(error => {
        console.log(error);
    })
}
const setRoom = (room) => {
    currentRoom.value = room;
    getMessages();
}

const getMessages = () => {
    axios.get(`/chat/room/${currentRoom.value.id}/messages`)
    .then(response =>{
        messages.value = response.data;
    })
    .catch(error => {
        console.log(error);
    })
}

//created
getRooms();


</script>

<template>
    <AppLayout title="Dashboard">
        <template #header>
            <h2 class="font-semibold text-xl text-gray-800 leading-tight">
                <chat-room-selection
                v-if="currentRoom"
                :rooms="chatRooms"
                :currentRoom="currentRoom"
                v-on:roomchanged="setRoom($event)"
                />
            </h2>
        </template>

        <div class="py-12">
            <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
                <div class="bg-white overflow-hidden shadow-xl sm:rounded-lg">
                    <message-container :messages="messages"/>
                    <input-message v-if="currentRoom !== null" :room="currentRoom" v-on:message-sent="getMessages()" />
                </div>
            </div>
        </div>
    </AppLayout>
</template>



