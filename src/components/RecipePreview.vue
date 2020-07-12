<template>
  <b-card class="overflow-hidden" style="max-width: 540px;">
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
              src="https://d1yjjnpx0p53s8.cloudfront.net/styles/logo-thumbnail/s3/032019/untitled-1_224.png?oBCe4GBjPfodIRuJvYTeaocmsqVT623z&itok=qRKFCAe2"
              title="vegetarian"
            />
            <img
              class="img-logo"
              v-if="recipe.vegan"
              src="data:image/png;base64,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"
              title="vegan"
            />
            <img
              class="img-logo"
              v-if="recipe.glutenFree"
              src="https://encrypted-tbn0.gstatic.com/images?q=tbn%3AANd9GcQM4CFKIKBP5UBhEkj3h-HnjZZWIpyNzqfKsQ&usqp=CAU"
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
            src="https://storage.needpix.com/thumbs/heart-plus.jpg"
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
            src="https://i.ya-webdesign.com/images/person-svg-heart-clipart-9.png"
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
