`<template>
  <b-container>
    <h3>{{ title }}:</h3>
    <b-row v-for="r in recipes" :key="r.id">
      <b-col>
        <RecipePreview class="recipePreview" :recipe="r" />
      </b-col>
    </b-row>
    <b-button href="#" variant="primary" v-on:click="refresh()">
      refresh
    </b-button>
  </b-container>
</template>

<script>
import RecipePreview from "./RecipePreview.vue";
export default {
  name: "RecipePreviewList",
  components: {
    RecipePreview,
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
  /*   data() {
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
          // "https://test-for-3-2.herokuapp.com/recipes/random" 
          this.$root.store.base_url + "/recipes/random"
        );

        //console.log(response);
        const recipes = response.data.randRecipes;
        this.recipes.push(...recipes);
        //console.log(this.recipes);
      } catch (error) {
        console.log(error);
      }
    },
  } ,*/
};
</script>

<style lang="scss" scoped>
.container {
  min-height: 400px;
}
</style>
