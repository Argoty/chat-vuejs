<template>
  <Auth v-if="!user" @onAuth="handleAuth" />
  <Chat
    v-else
    v-bind:username="user.username"
    v-bind:secret="user.secret"
  />
</template>

<script>
import Chat from './components/Chat.vue'
import Auth from './components/Auth.vue'

export default {
  name: 'App',
  components: {
    Chat,
    Auth
  },
  data() {
    return {
      user: undefined,
    };
  },
  methods: {
    handleAuth(user) {
      this.user = user;
    },
  },
};
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Belanosima&family=Rubik&display=swap');

* {
  font-family: 'Belanosima', sans-serif;
  font-family: 'Rubik', sans-serif;
}
body {
  margin: 0px;
}
</style>
