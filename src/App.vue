<template>
  <v-app>

    <v-content v-if="state == 'chat'">
        <Chat />
    </v-content>

    <v-content v-if="state == 'login'">
        <Login />
    </v-content>

    <v-content v-if="state == 'reg'">
        <Registration />
    </v-content>

  </v-app>
</template>

<script>
import Login from './components/LoginForm'
import Chat from './components/Chat'
import Registration from './components/Registration'

export default {
  name: 'App',
  components: {
        Login,
        Chat,
        Registration
  },
  data () {
    return {
      state: 'reg'
    }
  }
}
// state for displaying different components
</script>
