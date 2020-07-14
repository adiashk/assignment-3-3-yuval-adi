<template>
  <div>
    <div v-if="recipe">
      <b-row>
        <b-col>
          <div class="row">
            <!-- Team Member 1 -->
            <b-card-group deck>
              <!-- <div class="col-xl-6 col-md-10 mb-8"> -->
              <div class="c">
                <img :src="recipe.image" class="card-img-top" />
                <div class="card-body text-center">
                  <h1 class="card-title mb-0" style="color:black;">
                    {{ recipe.title }}
                  </h1>
                  <div class="card-text text-black-50">
                    <div>Ready in {{ recipe.readyInMinutes }} minutes</div>
                    <div>servings: {{ recipe.servings }}</div>

                    <b-col>
                      <img
                        class="img-logo"
                        v-if="recipe.vegetarian"
                        src="https://res.cloudinary.com/dqjzqwhv4/image/upload/v1594650915/veg_gbe3hi.png"
                        title="vegetarian"
                      />
                      <img
                        class="img-logo"
                        v-if="recipe.vegan"
                        src="https://www.gobrazilwines.com/wp-content/uploads/2019/01/GoBrazilWine_Vegan_Symbol-295x300.png"
                        title="vegan"
                      />
                      <img
                        class="img-logo"
                        v-if="recipe.glutenFree"
                        src="https://i.ya-webdesign.com/images/gluten-free-png-4.png"
                        title="gluten free"
                      />
                    </b-col>
                  </div>
                </div>
              </div>
              <!--                 </div>-->
            </b-card-group>
          </div>
        </b-col>
        <b-col>
          <div class="text-xl-left">
            <h1>Ingredients:</h1>
            <ul>
              <li v-for="(r, index) in recipe.ingredients" :key="index">
                {{ r.ingredient_name }}
                -
                {{ r.amount }}
                {{ r.unit }}
              </li>
            </ul>
          </div>
          <div class="text-left">
            <h1>Instructions:</h1>
            <ol>
              <li v-for="(s, index) in recipe.instructions" :key="index">
                {{ recipe.instructions[index].step_description }}
              </li>
            </ol>
          </div>
        </b-col>
      </b-row>

      <div class="recipe-body">
        <div class="wrapper"></div>
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
          this.$root.store.base_url + "/profile/getFullFamilyRecipe",
          {
            params: { recipe_id },
          }
        );

        console.log(response);
        // console.log("response.status: ", response.status);
        if (response.status !== 200) this.$router.replace("/NotFound");
      } catch (error) {
        console.log("error.response.status: ", error.response.status);
        this.$router.replace("/NotFound");
        return;
      }
      let {
        instructions,
        ingredients,
        aggregateLikes,
        readyInMinutes,
        image,
        title,
        vegetarian,
        vegan,
        glutenFree,
        servings,
      } = response.data.data;

      let _recipe = {
        instructions,
        ingredients,
        aggregateLikes,
        readyInMinutes,
        image,
        title,
        vegetarian,
        vegan,
        glutenFree,
        servings,
      };

      this.recipe = _recipe;
      console.log(this.recipe);
    } catch (error) {
      console.log(error);
    }

    try {
      if (this.$root.store.username) {
        console.log("recipe_id");
        console.log(recipe_id);

        addToWatched = await this.axios.post(
          this.$root.store.base_url + "/profile/addToWatchedRecipes",
          {
            recipe_id: recipe_id,
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
.img-logo {
  bottom: 8px;
  left: 0px;
  width: 12%;
}
.img-logo:hover {
  transform: scale(
    1.5
  ); /* (150% zoom - Note: if the zoom is too large, it will go outside of the viewport) */
}
.c {
  background-color: rgba(255, 255, 255, 0.815);
  /*   background-color: white;
 */
}
</style>
