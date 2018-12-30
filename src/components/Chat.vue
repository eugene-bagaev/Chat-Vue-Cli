<template>
    <ChatContainer v-on:logout="logout"  />
</template>

<script>
    import ChatContainer from '../components/ChatBody'

    export default {
        components: {
            ChatContainer
        },
        name: "Chat",
        methods: {
            logout(value) {
                if (value) {
                    this.$emit('logout', value);
                }
            }
        }
    }
</script>

<style scoped>

</style>