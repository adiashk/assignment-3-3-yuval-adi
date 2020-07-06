<template>
  <RecipePreviewList title="Last watched Recipes" :recipes="recipes" />
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
    this.get3ids();
  },
  methods: {
    async get3ids() {
      try {
        const response = await this.axios.get(
          this.$root.store.base_url + "/profile/get3WatchedRecipes"
        );

        const recipes = response.data.recipes;
        this.recipes.push(...recipes);
        //console.log(this.recipes);
      } catch (error) {
        console.log(error);
      }
    },
  },
};
</script>

<style></style>
