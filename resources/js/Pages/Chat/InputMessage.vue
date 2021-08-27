<template>
    <div>
        <div class="mt-5 flex">
            <input type="text" v-model="message" @keyup.enter="sendMessage()" class="w-full rounded-md" placeholder="Enter messages...">
            <button @click="sendMessage()" class="bg-indigo-500 hover:bg-indigo-400 px-3 ml-2 text-white font-bold rounded-md">Send</button>
        </div>
    </div>
</template>

<script>
import axios from 'axios';
export default {
    props: ['room'],
    data() {
        return {
            message: ''
        }
    },
    methods: {
        sendMessage() {
            if (this.message === '') {
                return;
            }
            axios.post('/chat/room/' + this.room.id + '/message', {
                message: this.message
            })
            .then(res => {
                if (res.status === 201) {
                    this.message = ''
                    this.$emit('message-send')
                }
            })
            .catch(err => console.log(err))
        }
    }

}
</script>

<style>

</style>