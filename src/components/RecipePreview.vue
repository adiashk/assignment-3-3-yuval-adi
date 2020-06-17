<template>
  <router-link
    :to="{ name: 'recipe', params: { recipeId: recipe.id } }"
    class="recipe-preview"
  >
    <b-card no-body class="overflow-hidden" style="max-width: 540px;">
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
            src="../assets/vegetarian.png"
          />
          <img class="img-logo" v-if="recipe.vegan" src="../assets/vegan.jpg" />
          <img
            class="img-logo"
            v-if="recipe.glutenFree"
            src="../assets/glutenFree.png"
          />
          <span v-if="getIsWhatchedRecipes" style="color:red">
            whatched before
          </span>
          <span v-else-if="!getIsWhatchedRecipes" style="color:green">
            not whatched before
          </span>
        </b-col>
        <b-col lg="4" class="pb-2">
          <b-button v-if="!getIsFavoriteRecipe" variant="success" size="sm"
            >add to favorites</b-button
          >
          <b-button v-else-if="getIsFavoriteRecipe" variant="danger" size="sm"
            >remove from favorites</b-button
          >
        </b-col>
      </b-row>
    </b-card>
  </router-link>
</template>

<script>
export default {
  mounted() {
    this.axios.get(this.recipe.image).then((i) => {
      this.image_load = true;
    });
  },
  data() {
    return {
      image_load: false,
      getIsWhatchedRecipes: false,
      getIsFavoriteRecipe: false,
    };
  },
  props: {
    recipe: {
      type: Object,
      required: true,
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
  width: 15%;
}
</style>
