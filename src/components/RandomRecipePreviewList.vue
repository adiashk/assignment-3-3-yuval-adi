<template>
  <RecipePreviewList title="Random Recipes" :recipes="recipes" />
</template>

<script>
import RecipePreviewList from "./RecipePreviewList.vue";
export default {
  //name: "RandomRecipePreviewList",
  components: {
    RecipePreviewList,
  },
  data() {
    return {
      recipes: [],
    };
  },
  mounted() {
    this.updateRecipes();
  },
  methods: {
    async updateRecipes() {
      try {
        const response = await this.axios.get(
          //"https://test-for-3-2.herokuapp.com/recipes/random"
          this.$root.store.base_url + "/recipes/random"
        );

        // console.log(response);
        const recipes = response.data.randRecipes;
        this.recipes.push(...recipes);
        // console.log(this.recipes);
      } catch (error) {
        console.log(error);
      }
    },
  },
};
</script>

<style lang="scss" scoped>
.container {
  min-height: 400px;
}
</style>
