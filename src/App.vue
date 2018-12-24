<template>
  <v-app>

    <v-content v-if="state == 'chat'">
        <Chat v-on:logout="logout" />
    </v-content>

    <v-content v-if="state == 'login'">
        <Login v-on:login="loginSuccess" v-on:signup="signup" />
    </v-content>

    <v-content v-if="state == 'reg'">
        <Registration />
    </v-content>

  </v-app>
</template>

<script>
import Login            from './components/LoginForm';
import Chat             from './components/Chat';
import Registration     from './components/Registration';

export default {
    name: 'App',
    components: {
        Login,
        Chat,
        Registration
    },
    data () {
        return {
            state: ''
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
        }
    },
    beforeMount() {
        if (this.$session.exists()) {
            console.log('Session exist');
            this.state = 'chat'
        } else {
            this.state = 'login'
        }
    }
}
// state for displaying different components
</script>
