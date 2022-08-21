
<template>
  <div id="app">
    <div id="nav">
      <router-link :to="{ name: 'main' }">Vue Recipes</router-link>  |  
      <router-link :to="{ name: 'search' }">Search</router-link>  |  
      <router-link :to="{ name: 'about' }">About</router-link>  |  
      <!-- {{ !$root.store.username }} -->
      <span v-if="!$root.store.username">
        Guest:
        <router-link :to="{ name: 'register' }">Register</router-link>  |  
        <router-link :to="{ name: 'login' }">Login</router-link>
      </span>
      <span v-else>
        {{ $root.store.username }}: <button @click="Logout">Logout</button>|
      </span>
    </div>
    <!-- <b-nanabr></b-nanabr> -->
    <router-view />
  </div>
</template>

<script>
export default {
  name: "App",
  methods: {
    Logout() {
      this.$root.store.logout();
      //
      this.axios.post(`http://localhost:3000/Logout`);
      this.axios.defaults.withCredentials = false; //remove cockie after logout
      //
      this.$root.toast("Logout", "User logged out successfully", "success");

      this.$router.push("/").catch(() => {
        this.$forceUpdate();
      });
    }
  }
};
</script>

<style lang="scss">
@import "@/scss/form-style.scss";


#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  min-height: 100vh;
  background-color:#a6ff0027;
  
  //background-image:"url('https://s3.envato.com/files/272509119/4H1A9758.jpg')";
    // width: '500px' , height: '200px'
    
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bolder;
  font-size: 20px;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}
</style>
