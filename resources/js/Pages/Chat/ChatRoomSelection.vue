<template>
    <div>
        <div class="flex justify-between items-center">
            <div class="font-bold text-indigo-500">
                #Active Chats in - <span class="text-indigo-700">{{ selected.name }}</span>
            </div>
            <div>
                <select v-model="selected" @change="$emit('changeChatRoom', selected)" class="rounded-md">
                    <option v-for="room in rooms" :key="room.id" :value="room">
                        {{ room.name }}    
                    </option>
                </select>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    props: ['rooms', 'currentRoom'],
    data() {
        return {
            selected: ''
        }
    },
    created() {
        this.selected = this.currentRoom
    }
}
</script>

<style>

</style>