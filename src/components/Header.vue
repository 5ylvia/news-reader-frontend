<template>
  <header>
    <h3>Header</h3>
    <router-link v-bind:to="'/'">Home</router-link>
    <router-link v-bind:to="'/about'">About</router-link>
    <router-link v-bind:to="'/register'">Register</router-link>
    <router-link v-bind:to="'/my-articles'" v-if="loggedIn === 'yes'">My Articles</router-link>
    <a href="#" v-if="loggedIn === 'yes'" @click.prevent="setLoggedOut">Log Out</a>
    <router-link v-else v-bind:to="'/login'">Log In</router-link>
    <p v-if="loggedIn === 'yes'" >Hello: {{ userEmail }}</p>

  </header>
</template>

<script>
import EventBus from "../eventBus.js"


export default {
  name: "MyHeader",
  data: function() {
    return {
      loggedIn: 'no',
      userEmail: null
    }
  },
  methods: {
    setLoggedOut: function() {
      localStorage.clear();
      localStorage.loggedIn = "no";
      localStorage.userEmail = null;
      EventBus.$emit("$userLogged");
      this.$router.push({ path:'/login' })
    },
    setLoggedIn: function() {
      this.loggedIn = localStorage.loggedIn;
      this.userEmail = localStorage.userEmail;
    }
  },
  mounted() {
    this.loggedIn = localStorage.loggedIn;
    this.userEmail = localStorage.userEmail;
    EventBus.$on("$userLogged", this.setLoggedIn)
  },

};
</script>

<style>
    .router-link-exact-active {
        color: green;
    }
</style>