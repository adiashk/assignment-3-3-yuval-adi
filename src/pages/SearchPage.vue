<template>
  <div class="container">
    <b-container class="bv">
      <!-- full page -->
      <b-row class="justify-content-md-center">
        <b-col></b-col>

        <b-col cols="12" md="auto">
          <!-- <h1 class="title">Search Page</h1> -->
          <br />
          <b-form id="form" @submit.prevent="search">
            <b-container class="bv-row1">
              <br />
              <b-row>
                <!-- <b-col col lg="2"></b-col> -->
                <b-col cols="12" md="auto">
                  <input
                    class="query"
                    v-model="searchQuery"
                    type="text"
                    placeholder="Enter search query"
                    id="q"
                    required
                  />
                  <b-button variant="success" type="submit">search</b-button>
                </b-col>
                <br />
                <br />
                <br />
                <!-- <b-col></b-col> -->
                <b-col>
                  <b-form-group id="number" label="Number of results:">
                    <b-form-radio-group
                      id="number_id"
                      :options="optionsNum"
                      v-model="selectedNum"
                      name="radio-inline"
                    ></b-form-radio-group>
                  </b-form-group>
                </b-col>
              </b-row>
            </b-container>

            <b-container class="bv-row2" id="dropdown">
              <b-row>
                <!-- drop down sorts -->
                <b-col cols="12" md="auto">
                  <b-dropdown
                    id="sort-dropdown"
                    text="  SORT  "
                    v-model="sort"
                    variant="success"
                    aria-expanded="false"
                    dropright
                  >
                    <b-dropdown-item @click="sortByHighPopularity"
                      >Sort by high popularity</b-dropdown-item
                    >
                    <div class="dropdown-divider"></div>
                    <b-dropdown-item @click="sortByLowPopularity"
                      >Sort by low popularity</b-dropdown-item
                    >
                    <div class="dropdown-divider"></div>
                    <b-dropdown-item @click="sortByHighPreparationgTime"
                      >Sort by high preparation time</b-dropdown-item
                    >
                    <div class="dropdown-divider"></div>
                    <b-dropdown-item @click="sortByLowPreparationgTime">
                      Sort by low preparation time</b-dropdown-item
                    >
                  </b-dropdown>
                  <br />
                  <br />
                  <!-- <br />
                  <br />
                  <br />
                  <br />
                  <br /> -->
                </b-col>
                <!-- end of drop down sorts -->
                <b-col col lg="2"></b-col>
                <b-col col lg="2"></b-col>
                <b-col col lg="2"></b-col>
                <!-- drop down filter -->
                <b-col cols="12" md="auto">
                  <b-dropdown
                    id="filter-dropdown"
                    text="    FILTERS   "
                    v-model="filter"
                    variant="success"
                    dropleft
                  >
                    <!-- <b-form-group id="cuisine" label="cuisine:"> -->
                    <b-form-group id="cuisine">
                      <b-form-select
                        id="cuisine_id"
                        :options="optionsCuisine"
                        v-model="selectedCuisine"
                      ></b-form-select>
                    </b-form-group>
                    <div class="dropdown-divider"></div>
                    <!-- <br /> -->
                    <!-- <b-form-group id="diet" label="diet:"> -->
                    <b-form-group id="diet">
                      <b-form-select
                        id="diet_id"
                        :options="optionsDiet"
                        v-model="selectedDiet"
                      ></b-form-select>
                      <div class="dropdown-divider"></div>
                      <!-- <br /> -->
                    </b-form-group>
                    <!-- <b-form-group id="Intolerance" label="Intolerance:"> -->
                    <b-form-group id="Intolerance">
                      <b-form-select
                        id="Intolerance_id"
                        :options="optionsIntolerance"
                        v-model="selectedIntolerance"
                      ></b-form-select>
                    </b-form-group>
                  </b-dropdown>
                  <br />
                  <br />
                  <!-- <br />
                  <br />
                  <br /> -->
                </b-col>
                <!-- end of drop dow filter -->
              </b-row>
            </b-container>
          </b-form>
          <RecipePreviewList
            title="Searched Recipes result"
            :recipes="recipes"
          />

          <b-alert
            v-model="showDismissibleAlert"
            variant="warning"
            dismissible
            style="text-align:center"
          >
            Sorry, No recipes found.
          </b-alert>
        </b-col>
        <b-col></b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
//button @click="sortByMakingTime">Sort by making time</button>
//  <button @click="sortByPopularity">Sort by popularity</button>

import RecipePreviewList from "../components/RecipePreviewList";

export default {
  components: {
    //RecipePreview,
    RecipePreviewList,
  },
  data() {
    return {
      name: "Search",
      showDismissibleAlert: false,
      filter: "",
      selectedCuisine: null,
      optionsCuisine: [
        //{ value: null, text: "Please select a cuisine" },
        { value: null, text: "cuisine" },
        { value: "African", text: "African" },
        { value: "American", text: "American" },
        { value: "British", text: "British" },
        { value: "Cajun", text: "Cajun" },
        { value: "Caribbean", text: "Caribbean" },
        { value: "Chinese", text: "Chinese" },
        { value: "Eastern European", text: "Eastern European" },
        { value: "European", text: "European" },
        { value: "French", text: "French" },
        { value: "German", text: "German" },
        { value: "Greek", text: "Greek" },
        { value: "Indian", text: "Indian" },
        { value: "Irish", text: "Irish" },
        { value: "Italian", text: "Italian" },
        { value: "Japanese", text: "Japanese" },
        { value: "Jewish", text: "Jewish" },
        { value: "Korean", text: "Korean" },
        { value: "Latin American", text: "Latin American" },
        { value: "Mediterranean", text: "Mediterranean" },
        { value: "Mexican", text: "Mexican" },
        { value: "Middle Eastern", text: "Middle Eastern" },
        { value: "Nordic", text: "Nordic" },
        { value: "Southern", text: "Southern" },
        { value: "Spanish", text: "Spanish" },
        { value: "Thai", text: "Thai" },
        { value: "Vietnamese", text: "Vietnamese" },
      ],
      selectedDiet: null,
      optionsDiet: [
        //{ value: null, text: "Please select a diet" },
        { value: null, text: "Diet" },
        { value: "Gluten Free", text: "Gluten Free" },
        { value: "Ketogenic", text: "Ketogenic" },
        { value: "Vegetarian", text: "Vegetarian" },
        { value: "Lacto-Vegetarian", text: "Lacto-Vegetarian" },
        { value: "Ovo-Vegetarian", text: "Ovo-Vegetarian" },
        { value: "Vegan", text: "Vegan" },
        { value: "Pescetarian", text: "Pescetarian" },
        { value: "Paleo", text: "Paleo" },
        { value: "Primal", text: "Primal" },
        { value: "Whole30", text: "Whole30" },
      ],
      selectedIntolerance: null,
      optionsIntolerance: [
        // { value: null, text: "Please select an intolerance" },
        { value: null, text: "Intolerance" },
        { value: "Dairy", text: "Dairy" },
        { value: "Egg", text: "Egg" },
        { value: "Gluten", text: "Gluten" },
        { value: "Grain", text: "Grain" },
        { value: "Peanut", text: "Peanut" },
        { value: "Seafood", text: "Seafood" },
        { value: "Sesame", text: "Sesame" },
        { value: "Shellfish", text: "Shellfish" },
        { value: "Soy", text: "Soy" },
        { value: "Sulfite", text: "Sulfite" },
        { value: "Tree nut", text: "Tree nut" },
        { value: "Wheat", text: "Wheat" },
      ],
      selectedNum: "5",
      optionsNum: [
        { value: "5", text: "5" },
        { value: "10", text: "10" },
        { value: "15", text: "15" },
      ],
      recipes: [],
      searchQuery: "",
      recipesLoaded: false,
      clickedSearch: false,

      form: {
        submitError: undefined,
      },
      sort: "",
      // sort: [
      //   "sortByHighPopularity",
      //   "sortByLowPopularity",
      //   "sortByHighPreparationgTime",
      //   "sortByLowPreparationgTime",
      // ],
    };
  },
  mounted() {
    this.updateLastSearch();
  },
  methods: {
    async searchQ() {
      try {
        let query = {};

        if (this.searchQuery != "") query.query = this.searchQuery;

        if (this.selectedCuisine != null) query.cuisine = this.selectedCuisine;

        if (this.selectedDiet != null) query.diet = this.selectedDiet;

        if (this.selectedIntolerance != null)
          query.intolerances = this.selectedIntolerance.value;

        query.number = this.selectedNum;

        const response = await this.axios.get(
          this.$root.store.base_url + "/recipes/search",
          {
            params: {
              query: query.query,
              cuisine: query.cuisine,
              diet: query.diet,
              intolerances: query.intolerance,
              number: query.number,
            },
          }
        );

        this.recipes = []; //empty stuck

        if (response.status !== 200) this.$router.replace("/NotFound");
        else {
          console.log("in search");
          this.showDismissibleAlert = false;

          const recipes = response.data.data;

          this.recipes.push(...recipes);
        }
        if (this.$root.store.username != "") {
          console.log("last");
          this.$root.store.lastSearch = this.recipes;
        }

        this.recipesLoaded = true;
      } catch (error) {
        console.log(error);
        this.showDismissibleAlert = true;
        this.recipes = [];
        this.form.submitError = undefined;
        return;
      }
    },
    search() {
      this.form.submitError = undefined;
      this.searchQ();
    },

    sortByHighPopularity() {
      this.recipes.sort(function(b, a) {
        return parseFloat(a.aggregateLikes) - parseFloat(b.aggregateLikes);
      });
    },
    sortByLowPopularity() {
      this.recipes.sort(function(a, b) {
        return parseFloat(a.aggregateLikes) - parseFloat(b.aggregateLikes);
      });
    },

    sortByHighPreparationgTime() {
      this.recipes.sort(function(b, a) {
        return parseFloat(a.readyInMinutes) - parseFloat(b.readyInMinutes);
      });
    },

    sortByLowPreparationgTime() {
      console.log("in sort");
      this.recipes.sort(function(a, b) {
        return parseFloat(a.readyInMinutes) - parseFloat(b.readyInMinutes);
      });
    },

    updateLastSearch() {
      if (this.$root.store.username != "") {
        this.recipes = this.$root.store.lastSearch;
        this.recipesLoaded = true;
      }
    },
  },
};
</script>

<style>
#q {
  background-color: white !important;
}
#form {
  background-color: rgba(255, 255, 255, 0.24) !important;
}

#filter-dropdown.bg-transparent #sort-dropdown.bg-transparent {
  background-color: green !important;
}
</style>
