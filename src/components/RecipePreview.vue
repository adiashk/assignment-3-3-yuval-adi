<template>
  <b-card bg-variant="white" text-variant="white" class="text-center">
    <!--   <b-card class="overflow-hidden" style="max-width: 540px;">
 -->
    <router-link
      :to="{ name: 'recipe', params: { recipeId: recipe.id } }"
      class="recipe-preview"
    >
      <b-card-body>
        <b-row no-gutters>
          <b-col md="6">
            <b-card-img :src="recipe.image" class="rounded-0"></b-card-img>
          </b-col>
          <b-col md="6">
            <b-card-body :title="recipe.title">
              <b-card-text>
                <li>{{ recipe.readyInMinutes }} minutes</li>
                <li>{{ recipe.aggregateLikes }} likes</li>
              </b-card-text>
            </b-card-body>
          </b-col>
        </b-row>
        <b-row no-gutters>
          <b-col md="6">
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
        </b-row>
      </b-card-body>
    </router-link>

    <b-row no-gutters v-if="$root.store.username">
      <b-col lg="4" class="pb-2">
        <span v-if="isWatchedRecipe_data" style="color:red">
          <img
            class="img-logo"
            src="https://olympiagraphics.com/wp-content/uploads/eye-logo-concept-1-1037x768.png"
            title="watched before"
          />
        </span>
        <span v-else-if="!isWatchedRecipe_data" style="color:green"> </span>
      </b-col>
      <b-col lg="4" class="pb-2">
        <div
          v-if="!isFavoriteRecipe_data"
          @click="addFavoritesRecipes"
          size="sm"
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
          size="sm"
          tag="b-button"
        >
          <img
            class="img-logo"
            src="https://encrypted-tbn0.gstatic.com/images?q=tbn%3AANd9GcT_A8lf5dhhsYzwcNApd35v0WiuM7q5fxBSaQ&usqp=CAU"
            title="delete from favorites recipes"
          />
        </div>
      </b-col>
    </b-row>
  </b-card>
</template>

<script>
export default {
  mounted() {
    this.getIsWatchedRecipe();
    this.getIsFavoriteRecipe();
  },
  methods: {
    async getIsWatchedRecipe() {
      try {
        if (this.$root.store.username) {
          const response = await this.axios.get(
            this.$root.store.base_url + "/profile/getIsWatchedRecipe",
            {
              params: { recipe_id: this.recipe.id },
            }
          );
          // console.log("response=" + response.data + " id= " + this.recipe.id);
          // console.log(response.data);
          this.isWatchedRecipe_data = response.data;
          // console.log(this.isWhatchedRecipe);
        } else this.isWatchedRecipe_data = false;
      } catch (error) {
        console.log(error);
      }
    },
    async getIsFavoriteRecipe() {
      try {
        if (this.$root.store.username) {
          const response = await this.axios.get(
            this.$root.store.base_url + "/profile/getIsFavoriteRecipe",
            {
              params: { recipe_id: this.recipe.id },
            }
          );
          //  console.log(response.data);
          this.isFavoriteRecipe_data = response.data;
          // console.log(this.isFavoriteRecipe);
        } else this.isFavoriteRecipe_data = false;
      } catch (error) {
        console.log(error);
      }
    },
    async addFavoritesRecipes() {
      // console.log("add fav");
      // console.log(this.recipe.id);
      try {
        if (this.$root.store.username) {
          const response = await this.axios.post(
            this.$root.store.base_url + "/profile/addFavoritesRecipes ",
            {
              recipe_id: this.recipe.id,
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
              recipe_id: this.recipe.id,
            }
          );

          this.isFavoriteRecipe_data = false;
        }
      } catch (error) {
        console.log(error);
      }
    },
  },
  data() {
    return {
      isWatchedRecipe_data: this.isWatchedRecipe,
      isFavoriteRecipe_data: this.isFavoriteRecipe,
    };
  },
  props: {
    recipe: {
      type: Object,
      required: true,
    },
    isWatchedRecipe: {
      type: Boolean,
      required: false,
      default() {
        return false;
      },
    },
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
.recipe-preview {
  display: inline-block;
  width: 90%;
  height: 100%;
  position: relative;
  margin: 10px 10px;
}
.recipe-preview > .recipe-body {
  width: 100%;
  height: 200px;
  position: relative;
}
.recipe-preview .recipe-body .recipe-image {
  margin-left: auto;
  margin-right: auto;
  margin-top: auto;
  margin-bottom: auto;
  display: block;
  width: 70%;
  height: auto;
  -webkit-background-size: cover;
  -moz-background-size: cover;
  background-size: cover;
}
.recipe-preview .recipe-footer {
  width: 100%;
  height: 50%;
  overflow: hidden;
}
.recipe-preview .recipe-footer .recipe-title {
  padding: 10px 10px;
  width: 100%;
  font-size: 12pt;
  text-align: left;
  white-space: nowrap;
  overflow: hidden;
  -o-text-overflow: ellipsis;
  text-overflow: ellipsis;
}
.recipe-preview .recipe-footer ul.recipe-overview {
  padding: 5px 10px;
  width: 100%;
  display: -webkit-box;
  display: -moz-box;
  display: -webkit-flex;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-flex: 1;
  -moz-box-flex: 1;
  -o-box-flex: 1;
  box-flex: 1;
  -webkit-flex: 1 auto;
  -ms-flex: 1 auto;
  flex: 1 auto;
  table-layout: fixed;
  margin-bottom: 0px;
}
.recipe-preview .recipe-footer ul.recipe-overview li {
  -webkit-box-flex: 1;
  -moz-box-flex: 1;
  -o-box-flex: 1;
  -ms-box-flex: 1;
  box-flex: 1;
  -webkit-flex-grow: 1;
  flex-grow: 1;
  width: 90px;
  display: table-cell;
  text-align: center;
}
.img-logo {
  bottom: 8px;
  left: 0px;
  width: 20%;
}
.img-logo:hover {
  transform: scale(
    1.5
  ); /* (150% zoom - Note: if the zoom is too large, it will go outside of the viewport) */
}
</style>
