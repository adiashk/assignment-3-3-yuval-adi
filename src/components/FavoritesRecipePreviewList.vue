<template>
  <RecipePreviewList title="Favorites Recipes" :recipes="recipes" />
</template>

<script>
import RecipePreviewList from "./RecipePreviewList.vue";
export default {
  components: {
    RecipePreviewList,
  },
  data() {
    return {
      recipes: [],
    };
  },
  mounted() {
    this.getFavoritesRecipes();
  },
  methods: {
    async getFavoritesRecipes() {
      try {
        const response = await this.axios.get(
          this.$root.store.base_url + "/profile/getFavoritesRecipes"
        );

        const recipes = response.data.recipes;
        this.recipes.push(...recipes);
      } catch (error) {
        console.log(error);
      }
    },
  },
};
</script>

<style></style>
