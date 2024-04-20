<script setup>
import AppLayout from '@/Layouts/AppLayout.vue';
import MessageContainer from '@/Pages/Chat/messageContainer.vue';
import InputMessage from "@/Pages/Chat/inputMessage.vue";
import ChatRoomItem from '@/Pages/Chat//chatRoomItem.vue';
import { ref, watch, reactive } from 'vue';

//Data Property
const chatRooms = ref([]);
const currentRoom = ref(null);
const messages = ref([]);
let isModalOpen = ref(false);
const newChatName = ref('');

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

const createChatRoom = () => {
    if(newChatName.value == ''){
        return
    }

    axios.post(`/chat/room/create`, {
        name: newChatName.value
    }).then(response => {
        getRooms();
        toggleModal();
    }).catch(error => {
        console.log(error);
    })
}

const toggleModal = () => {
    newChatName.value = '';
    isModalOpen.value = !isModalOpen.value;
}

//created
getRooms();


</script>

<template>
    <AppLayout title="Dashboard">

        <div class="pt-20 h-[44rem]">
            <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
                <div class="bg-white h-[38rem] overflow-hidden shadow-xl sm:rounded-lg flex">
                    <div class="w-1/3 border flex flex-col">

                        <button @click="toggleModal" class=" bg-pink-500 hover:bg-pink-700 text-white font-bold m-3 py-2 px-4 rounded">Create Room +</button>
                        <hr class="border-2">
                        
                        <div class="overflow-y-auto">
                            <div v-for="(chatRoom, index) in chatRooms" :key="index">
                                <chat-room-item :chatRoom="chatRoom" :currentRoom="currentRoom" v-on:roomchanged="setRoom($event)"></chat-room-item>
                            </div>

                        </div>
                        <div v-if="isModalOpen" class="fixed inset-0 flex items-center justify-center bg-black bg-opacity-50 z-2 mb-4">
                            <div class="bg-white p-4 w-96 rounded shadow-xl">
                                <h2 class="text-lg font-bold mb-4 border-b-4">Create New Chat Room</h2>
                                <div class="mb-8">
                                    <label class="block mb-2 text-sm font-medium text-grey-900">Enter Chat Name</label>
                                    <input type="text" v-model="newChatName" class="block w-full border border-gray-300 text-grey-900 text-sm rounded-lg focus:ring-pink-500 focus:border-pink-500"/>
                                </div>
                                
                                <div class="float-right">
                                    <button @click="toggleModal" class="bg-gray-300 hover:bg-gray-400 text-gray-800 font-bold py-2 px-4 me-2 rounded">
                                    Close
                                </button>

                                <button @click="createChatRoom" class="bg-pink-500 hover:bg-pink-700 text-white font-bold py-2 px-4 rounded">
                                    Create
                                </button>
                                </div>
                                


                            </div>
                        </div>
                    </div>

                    

                    <div class="h-full w-full">
                        <message-container :messages="messages"/>
                        <input-message v-if="currentRoom !== null" :room="currentRoom" v-on:message-sent="getMessages()" />
                    </div>
                    
                </div>
            </div>
        </div>
    </AppLayout>
</template>

 
   











