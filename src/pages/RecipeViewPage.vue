<template>
  <div>
    <div v-if="recipe">
      <b-row>
        <!--         <b-col>
          <div class="text-xl-left">
            <h1>Ingredients:</h1>
            <ul>
              <li v-for="(r, index) in recipe.ingridients" :key="index">
                {{ r }}
              </li>
            </ul>
          </div>
        </b-col> -->
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
                    <div>Likes: {{ recipe.aggregateLikes }} likes</div>
                    <div>servings: {{ recipe.servings }}</div>

                    <b-col>
                      <img
                        class="img-logo"
                        v-if="recipe.vegetarian"
                        src="https://res.cloudinary.com/ddmhcwaul/image/upload/v1594558862/Vegetarian-2-512_jzy0lc.png"
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
                    <br />
                    <b-row no-gutters v-if="$root.store.username">
                      <b-col>
                        <div
                          v-if="!isFavoriteRecipe_data"
                          @click="addFavoritesRecipes"
                          size="lg"
                          tag="b-button"
                        >
                          <img
                            class="img-logo"
                            src="https://i.ya-webdesign.com/images/person-svg-heart-clipart-9.png"
                            title="add to favorites recipes"
                          />
                        </div>
                        <div
                          v-else-if="isFavoriteRecipe_data"
                          @click="deleteFavoritesRecipes"
                          size="lg"
                          tag="b-button"
                        >
                          <img
                            class="img-logo"
                            src="https://upload.wikimedia.org/wikipedia/commons/thumb/f/f1/Heart_coraz%C3%B3n.svg/1200px-Heart_coraz%C3%B3n.svg.png"
                            title="delete from favorites recipes"
                          />
                        </div>
                      </b-col>
                    </b-row>
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
              <li v-for="(r, index) in recipe.ingridients" :key="index">
                {{ r }}
              </li>
            </ul>
          </div>
          <div class="text-left">
            <h1>Instructions:</h1>
            <ol>
              <li v-for="(s, index) in recipe.instructions" :key="index">
                {{ recipe.instructions[index] }}
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
      isFavoriteRecipe_data: this.isFavoriteRecipe,
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
        ingridients,
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
        ingridients,
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
  mounted() {
    this.getIsFavoriteRecipe();
  },
  methods: {
    async getIsFavoriteRecipe() {
      try {
        if (this.$root.store.username) {
          const response = await this.axios.get(
            this.$root.store.base_url + "/profile/getIsFavoriteRecipe",
            {
              params: { recipe_id: this.$route.params.recipeId },
            }
          );
          this.isFavoriteRecipe_data = response.data;
        } else this.isFavoriteRecipe_data = false;
      } catch (error) {
        console.log(error);
      }
    },
    async addFavoritesRecipes() {
      try {
        if (this.$root.store.username) {
          const response = await this.axios.post(
            this.$root.store.base_url + "/profile/addFavoritesRecipes ",
            {
              recipe_id: this.$route.params.recipeId,
            }
          );

          this.isFavoriteRecipe_data = true;
        }
      } catch (error) {
        console.log(error);
      }
    },
    async deleteFavoritesRecipes() {
      try {
        if (this.$root.store.username) {
          const response = await this.axios.post(
            this.$root.store.base_url + "/profile/deleteFavoritesRecipes ",
            {
              recipe_id: this.$route.params.recipeId,
            }
          );

          this.isFavoriteRecipe_data = false;
        }
      } catch (error) {
        console.log(error);
      }
    },
  },
  props: {
    isFavoriteRecipe: {
      type: Boolean,
      required: false,
      default() {
        return false;
      },
    },
  },
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
