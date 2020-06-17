<template>
  <b-navbar type="dark" variant="info">
    <router-link tag="b-navbar-brand" :to="{ name: 'main' }"
      >Vue Recipes</router-link
    >|
    <router-link tag="b-navbar-item" :to="{ name: 'search' }"
      >Search</router-link
    >|
    {{ !$root.store.username }}
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
      {{ $root.store.username }}: <button @click="Logout">Logout</button>|
    </span>
  </b-navbar>
</template>

<script>
export default {};
</script>

<style>
</style>
