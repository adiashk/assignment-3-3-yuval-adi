<template>
  <b-container class="container">
    <b-row>
      <b-col>
        <h1 class="title text-center"></h1>
      </b-col>
    </b-row>
    <b-row>
      <b-col>
        <RandomRecipePreviewList
          title="Random Recipes"
          class="RandomRecipes center"
        />
      </b-col>
      <b-col v-if="!$root.store.username">
        <login-page></login-page>
      </b-col>
      <b-col v-else>
        <LastWatchedRecipePreviewList
          title="Last Viewed Recipes"
          class="LastWatchedRecipes center"
        ></LastWatchedRecipePreviewList>
      </b-col>
    </b-row>
    <b-button href="#" variant="primary" v-on:click="refresh()">
      refresh
    </b-button>
  </b-container>
</template>

<script>
import RandomRecipePreviewList from "../components/RandomRecipePreviewList";
import LastWatchedRecipePreviewList from "../components/LastWatchedRecipePreviewList";
import LoginPage from "../pages/LoginPage";
export default {
  components: {
    RandomRecipePreviewList,
    LastWatchedRecipePreviewList,
    LoginPage,
  },
  methods: {
    refresh: function(msg) {
      window.location.reload();
    },
  },
};
</script>

<style lang="scss" scoped>
.RandomRecipes {
  margin: 10px 0 10px;
}
.LastWatchedRecipes {
  margin: 10px 0 10px;
}
.blur {
  -webkit-filter: blur(5px); /* Safari 6.0 - 9.0 */
  filter: blur(2px);
}
::v-deep .blur .recipe-preview {
  pointer-events: none;
  cursor: default;
}
</style>
