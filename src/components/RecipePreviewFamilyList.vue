<template>
  <b-container>
    <h3>{{ title }}:</h3>
    <b-row v-for="r in recipes" :key="r.id">
      <b-col>
        <RecipePreviewFamily class="recipePreview" :recipe="r" />
      </b-col>
    </b-row>
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
};
</script>

<style lang="scss" scoped>
.container {
  min-height: 400px;
}
</style>
