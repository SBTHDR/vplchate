<template>
    <app-layout title="Dashboard">
        <template #header>
            <h2 class="font-semibold text-xl text-gray-800 leading-tight">
                <ChatRoomSelection v-if="currentRoom.id" :rooms="chatRooms" :currentRoom="currentRoom" 
                v-on:changeChatRoom="setRoom($event)"/>
            </h2>
        </template>

        <div class="py-12">
            <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
                <div class="bg-white overflow-hidden shadow-xl sm:rounded-lg p-3">
                    <MessageContainer :messages="messages" />
                    <InputMessage :room="currentRoom" v-on:message-send="getMessages()"/>
                </div>
            </div>
        </div>
    </app-layout>
</template>

<script>
    import AppLayout from '@/Layouts/AppLayout.vue'
    import MessageContainer from './MessageContainer.vue'
    import InputMessage from './InputMessage.vue'
    import ChatRoomSelection from './ChatRoomSelection.vue'

    export default {
        components: {
            AppLayout,
            MessageContainer,
            InputMessage,
            ChatRoomSelection
        },
        data() {
           return {
                chatRooms: [],
                currentRoom: [],
                messages: []
            }
        },
        watch: {
            currentRoom(val, oldVal) {
                if( oldVal.id) {
                    this.disconnect(oldVal); 
                }
                this.connect();
            }
        },
        methods: {
            connect() {
                if (this.currentRoom.id) {
                    let vm = this;
                    this.getMessages();
                    window.Echo.private("chat." + this.currentRoom.id)
                    .listen('NewChatMessage', e => {
                        vm.getMessages();
                    })
                }
            },
            disconnect(room) {
                window.Echo.leave("chat." + room.id);
            },
            getRooms() {
                axios.get('/chat/rooms')
                    .then(res => {
                        this.chatRooms = res.data
                        this.setRoom(res.data[0])
                    })
                    .catch(err => console.log(err))
            },
            setRoom(room) {
                this.currentRoom = room
            },
            getMessages() {
                axios.get('/chat/room/' + this.currentRoom.id + '/messages')
                    .then(res => {
                        this.messages = res.data
                    })
                    .catch(err => console.log(err))
            }
        },
        created() {
            this.getRooms()
        }
    }
</script>
                
