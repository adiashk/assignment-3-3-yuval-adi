<template>
  <b-container>
    <h3>{{ title }}:</h3>
    <b-row v-for="r in recipes" :key="r.id">
      <b-col>
        <RecipePreviewFamily class="recipePreview" :recipe="r" />
      </b-col>
    </b-row>
    <b-button href="#" variant="primary" v-on:click="refresh()">
      refresh
    </b-button>
  </b-container>
</template>

<script>
import RecipePreviewFamily from "./RecipePreviewFamily.vue";
export default {
  components: {
    RecipePreviewFamily,
  },
  props: {
    title: {
      type: String,
      required: true,
    },
    recipes: {
      type: Array,
      required: true,
    },
  },
  methods: {
    refresh: function(msg) {
      window.location.reload();
    },
  },
};
</script>

<style lang="scss" scoped>
.container {
  min-height: 400px;
}
</style>
