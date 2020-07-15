<template>
  <b-navbar type="dark" variant="dark">
    <div
      class="navbar-collapse collapse w-100 order-1 order-md-0 dual-collapse2"
    >
      <router-link id="main" tag="b-navbar-brand" :to="{ name: 'main' }"
        >Home</router-link
      >|

      <router-link tag="b-navbar-brand" :to="{ name: 'search' }"
        >Search</router-link
      >|
      <router-link tag="b-navbar-brand" :to="{ name: 'About' }"
        >About</router-link
      >|
    </div>
    <span v-if="!$root.store.username">
      <div
        id="hello"
        class="navbar-collapse collapse w-100 order-3 dual-collapse2"
      >
        <router-link tag="b-navbar-brand" right :to="{ name: 'register' }"
          >Hello guest: Register</router-link
        >|
        <router-link tag="b-navbar-brand" :to="{ name: 'login' }"
          >Login</router-link
        >|
      </div>
    </span>

    <span v-else>
      <b-navbar-nav>
        <div class="navbar-collapse collapse w-100 order-3 dual-collapse2">
          <b-nav-item-dropdown
            id="Personal-dropdown"
            text="Personal"
            data-toggle="dropdown"
            variant="dark"
            aria-haspopup="true"
            right
          >
            <b-dropdown-item to="/Favorites">My Favorites</b-dropdown-item>
            <b-dropdown-item to="/Personal">My Recipes</b-dropdown-item>
            <b-dropdown-item to="/Family">My Family Recipes</b-dropdown-item>
          </b-nav-item-dropdown>

          <b-nav-item-dropdown
            id="Profile-dropdown"
            div
            :text="$root.store.username"
            data-toggle="dropdown"
            aria-haspopup="true"
            style="right:0"
            variant="success"
            right
          >
            <b-dropdown-item to="/profile">Profile</b-dropdown-item>
            <b-dropdown-item-button @click="Logout"
              >sign out</b-dropdown-item-button
            >
          </b-nav-item-dropdown>
        </div>
      </b-navbar-nav>
    </span>
  </b-navbar>
</template>

<script>
// <b-icon align="bottom" shift-v="-10.3" icon="person-fill" ></b-icon>
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
  color: red;
}

// .Personal-dropdown {
//   font: outline;
//   color: white;
// }
</style>
