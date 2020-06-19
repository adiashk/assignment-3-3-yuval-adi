<template>
  <b-navbar type="dark" variant="info">
    <router-link tag="b-navbar-brand" :to="{ name: 'main' }"
      >Vue Recipes</router-link
    >|
    <router-link tag="b-navbar-item" :to="{ name: 'search' }"
      >Search</router-link
    >|
   
    <span v-if="!$root.store.username">
      Guest: 
      <router-link tag="b-navbar-item" :to="{ name: 'register' }"
        >Register</router-link
      >|
      <router-link tag="b-navbar-item" :to="{ name: 'login' }"
        >Login</router-link
      >|
    </span>
    <span v-else>
       Hello {{ $root.store.username }}: 
      <button @click="Logout">Logout</button>|
    </span>
  </b-navbar>
</template>

<script>
//import { required } from "vuelidate/lib/validators";
export default {
  name: "Logout",
  data() {
    return {
      form: {
        username: "",
      }
    };
  },
  methods: {

    async Logout() {
      try {
        const response = await this.axios.post(

          this.$root.store.base_url + "/user/Logout",
          {
          }
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
    }
  }
};
</script>
<style lang="scss" scoped>
.container {
  max-width: 400px;
}
</style>
