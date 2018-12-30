<template>
    <div></div>
</template>

<script>

    import Vue          from 'vue';
    import WebSocket    from 'vue-native-websocket';
    import VueSession   from 'vue-session';

    Vue.use(WebSocket, "ws://localhost:8080/chat_entry_point");
    Vue.use(VueSession);

    export default {
        name: "WebSocket"
    }
</script>

<style scoped>

</style>