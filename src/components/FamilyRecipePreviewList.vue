<template>
  <!--   <b-container>
    <h3>{{ title }}:</h3>
    <b-row v-for="r in recipes" :key="r.id">
      <b-col>
        <RecipePreviewFamilyList class="recipePreview" :recipe="r" />
      </b-col>
    </b-row>
  </b-container> -->
  <RecipePreviewFamilyList title="Family Recipes" :recipes="recipes" />
</template>

<script>
import RecipePreviewFamilyList from "./RecipePreviewFamilyList.vue";
export default {
  components: {
    RecipePreviewFamilyList,
  },
  props: {
    title: {
      type: String,
      required: true,
    },
  },
  data() {
    return {
      recipes: [],
    };
  },
  mounted() {
    this.getFamilyRecipes();
  },
  methods: {
    async getFamilyRecipes() {
      try {
        const response = await this.axios.get(
          this.$root.store.base_url + "/profile/getFamilyRecipes"
        );
        console.log(response);
        const recipes = response.data.familyRecipes;
        this.recipes.push(...recipes);
        console.log(this.recipes);
      } catch (error) {
        console.log(error);
      }
    },
  },
};
</script>

<style></style>
