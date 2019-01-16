<template>
    <div></div>
</template>

<script>

    import Vue          from 'vue';
    import WebSocket    from 'vue-native-websocket';
    import VueSession   from 'vue-session';
    import VueCookie    from 'vue-cookies';

    Vue.use(WebSocket, "ws://104.248.243.134:8080/chatServer/chat_entry_point");
    Vue.use(VueSession);
    Vue.use(VueCookie);

    export default {
        name: "WebSocket"
    }
</script>

<style scoped>

</style>