<template>
  <!--   <b-container>
    <h3>{{ title }}:</h3>
    <b-row v-for="r in recipes" :key="r.id">
      <b-col>
        <RecipePreview class="recipePreview" :recipe="r" />
      </b-col>
    </b-row>
  </b-container> -->

  <RecipePreviewList title="Last watched Recipes" :recipes="recipes" />
</template>

<script>
//import RecipePreview from "./RecipePreview.vue";
import RecipePreviewList from "./RecipePreviewList.vue";
export default {
  components: {
    //RecipePreview,\
    RecipePreviewList,
  },
  /*   props: {
    title: {
      type: String,
      required: true, 
    },
  }, */
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
        //console.log(this.recipes);
      } catch (error) {
        console.log(error);
      }
    },
  },
};
</script>

<style></style>
