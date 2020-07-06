<template>
  <div class="container">
    <div v-if="recipe">
      <div class="recipe-header mt-3 mb-4">
        <h1>{{ recipe.title }}</h1>
        <img :src="recipe.image" class="center" />
      </div>
      <div class="recipe-body">
        <div class="wrapper">
          <div class="wrapped">
            <div class="mb-3">
              <div>Ready in {{ recipe.readyInMinutes }} minutes</div>
              <div>Likes: {{ recipe.aggregateLikes }} likes</div>
            </div>

            Ingredients:
            <ul v-for="(r, index) in recipe.ingridients" :key="index">
              {{
                index + 1
              }}
              :
              {{
                r
              }}
            </ul>
          </div>
          <div class="wrapped">
            Instructions:
            <ol>
              <li v-for="(s, index) in recipe.instructions" :key="index">
                {{ recipe.instructions[index] }}
              </li>
            </ol>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      recipe: null,
    };
  },
  async created() {
    let recipe_id = this.$route.params.recipeId;

    try {
      let addToWatched;
      let response;
      try {
        response = await this.axios.get(
          this.$root.store.base_url + "/recipes/displayFullRecipe",
          {
            params: { recipe_id },
          }
        );

        //console.log(response);
        // console.log("response.status: ", response.status);
        if (response.status !== 200) this.$router.replace("/NotFound");
      } catch (error) {
        console.log("error.response.status: ", error.response.status);
        this.$router.replace("/NotFound");
        return;
      }
      let {
        instructions,
        ingridients,
        aggregateLikes,
        readyInMinutes,
        image,
        title,
      } = response.data.data;

      let _recipe = {
        instructions,
        ingridients,
        aggregateLikes,
        readyInMinutes,
        image,
        title,
      };

      this.recipe = _recipe;
      console.log(this.recipe);
    } catch (error) {
      console.log(error);
    }

    try {
      if (this.$root.store.username) {
        console.log(recipe_id);

        addToWatched = await this.axios.post(
          this.$root.store.base_url + "/profile/addToWatchedRecipes",
          {
            params: { recipe_id },
          }
        );
        console.log(addToLastWatched);
      }
    } catch (error) {
      console.log(error);
    }
  },
  /*   mounted() {
  //  this.addWatchedRecipe();
    //this.getIsFavoriteRecipe();
  }, */
  /*   methods: {
    async addWatchedRecipe() {
      try {
        // if (this.$root.store.username) {
        const response = await this.axios.post(
          this.$root.store.base_url + "/profile/addToWatchedRecipes",
          {
            params: { recipe_id: this.$route.params.recipeId },
          }
        );
        console.log(response.status);
        //   }
      } catch (error) {
        console.log(error);
      }
    },
  }, */
};
</script>

<style scoped>
.wrapper {
  display: flex;
}
.wrapped {
  width: 50%;
}
.center {
  display: block;
  margin-left: auto;
  margin-right: auto;
  width: 50%;
}
/* .recipe-header{

} */
</style>
