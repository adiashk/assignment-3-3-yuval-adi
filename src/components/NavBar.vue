<template>

  <b-navbar type="dark" variant="info">
    <router-link tag="b-navbar-brand" :to="{ name: 'main' }"
      >Vue Recipes</router-link
    >|
    <router-link tag="b-navbar-item" :to="{ name: 'search' }"
      >Search</router-link
    >|

    <span v-if="!$root.store.username">
      <router-link tag="b-navbar-item" :to="{ name: 'register' }"
        >Register</router-link
      >|
      <router-link tag="b-navbar-item" :to="{ name: 'login' }"
        >Login</router-link
      >|
      
    </span>

    <span v-else>
      
      <b-nav-item-dropdown id="Personal-dropdown" text="Personal" data-toggle="dropdown" aria-haspopup="true" right>
      <b-dropdown-item to="/Favorites">My Favorites</b-dropdown-item>
      <b-dropdown-item to="/Personal">My Recipes</b-dropdown-item>
      <b-dropdown-item to="/Family">My Family Recipes</b-dropdown-item>
    </b-nav-item-dropdown>

     
      <b-nav-item-dropdown id="Profile-dropdown" div :text="$root.store.username" data-toggle="dropdown" aria-haspopup="true" left>
      <b-dropdown-item to="/profile">Profile</b-dropdown-item>
      <b-dropdown-item-button @click="Logout">sign out</b-dropdown-item-button>
    </b-nav-item-dropdown>


      
    </span>
  </b-navbar>
</template>

<script>
// <button @click="Logout">Logout</button>|
//import { required } from "vuelidate/lib/validators";
export default {
  name: "Logout",
  data() {
    return {
      form: {
        username: "",
      },
    };
  },
  methods: {
    async Logout() {
      try {
        const response = await this.axios.post(
          this.$root.store.base_url + "/user/Logout",
          {}
        );

        console.log(this.$root.store.logout);
        this.$root.store.login(this.form.username);
        // this.$router.push("/");
      } catch (err) {
        console.log(err.response);
        this.form.submitError = err.response.data.message;
      }
    },
    onLogout() {
      this.Logout();
    },
  },
};
</script>
<style lang="scss" scoped>
.container {
  max-width: 400px;
}
</style>
