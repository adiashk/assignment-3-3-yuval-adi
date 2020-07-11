<template>
  <RecipePreviewPersonalList title="personal Recipes" :recipes="recipes" />
</template>

<script>
import RecipePreviewPersonalList from "./RecipePreviewPersonalList.vue";
export default {
  //name: "RandomRecipePreviewList",
  components: {
    RecipePreviewPersonalList,
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
          this.$root.store.base_url + "/profile/getMyRecipes"
        );

        // console.log(response);
        const recipes = response.data.personalRecipes;
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
