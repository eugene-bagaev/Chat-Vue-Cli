<template>
  <v-app>

    <v-content v-if="state == 'chat'">
        <Chat v-on:logout="logout" />
    </v-content>

    <v-content v-if="state == 'login'">
        <Login v-on:login="loginSuccess" v-on:signup="signup" v-on:signin="signin" />
    </v-content>

    <v-content v-if="state == 'reg'">
        <Registration v-on:signin="signin" v-on:login="loginSuccess" />
    </v-content>

    <v-content>
        <WebSocket />
    </v-content>

  </v-app>
</template>

<script>
import Login            from './components/LoginForm';
import Chat             from './components/Chat';
import Registration     from './components/Registration';
import WebSocket        from './components/WebSocket';

export default {
    name: 'App',
    components: {
        WebSocket,
        Login,
        Chat,
        Registration
    },
    data () {
        return {
            state: 'login'
        }
    },
    methods: {
        loginSuccess(value) {
            if (value) {
                this.state = 'chat';
            }
        },
        logout(value) {
            if (value) {
                this.state = 'login'
            }
        },
        signup(value) {
            if (value) {
                this.state = 'reg'
            }
        },
        signin(value) {
            if (value) {
                this.state = 'login'
            }
        }
    },
    beforeCreate() { }
}
</script>
