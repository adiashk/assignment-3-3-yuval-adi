<template>
  <b-card class="overflow-hidden" style="max-width: 540px;">
    <router-link
      :to="{ name: 'PersonalRecipe', params: { recipeId: recipe.id } }"
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
              </b-card-text>
            </b-card-body>
          </b-col>
        </b-row>
      </b-card-body>
    </router-link>
  </b-card>
</template>

<script>
export default {
  data() {
    return {};
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
