<template>
    <div class=" h-screen relative">
        <div class="absolute bottom-0">
            <div v-for="message in messages" :key="message.id">
                <MessageItem :message="message"/>
            </div>
        </div>
    </div>
</template>

<script>
import MessageItem from './MessageItem.vue'
export default {
  components: { MessageItem },
    props: ['messages']

}
</script>

<style>

</style>