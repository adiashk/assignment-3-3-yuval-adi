<template>
<div class="search_container">
  <div class="container">
    <h1 class="title">Search Page</h1>
    <input
                v-model="searchQuery"
                type="text"
                placeholder="Enter search query"
                required
        />
        
        <b-button @click="search">search</b-button>
        <b-form-group label="Number of results:">
            <b-form-radio-group
                    v-model="selectedNum"
                    :options="optionsNum"
                    name="radio-inline"
            ></b-form-radio-group>
        </b-form-group>

         <b-form-group label="cuisine:">
            <b-form-select v-model="selectedCuisine" :options="optionsCuisine"></b-form-select>
         </b-form-group>
            <!-- <br /> -->
        <b-form-group label="diet:">
            <b-form-select  v-model="selectedDiet" :options="optionsDiet"></b-form-select>
            <!-- <br /> -->
         </b-form-group>
         <b-form-group label="Intolerance:">
            <b-form-select  v-model="selectedIntolerance" :options="optionsIntolerance"></b-form-select>
        </b-form-group>
        <button @click="sortByMakingTime">Sort by making time</button>
        <button @click="sortByPopularity">Sort by popularity</button>
        <b-alert  v-model="showDismissibleAlert" variant="warning" dismissible style="text-align:center">
            No recipes found for the inserted query
        </b-alert>

        <RecipePreview style="float:left;"
                v-for="r in recipes"
                :id="r.id"
                :title="r.title"
                :ready-in-minutes="r.readyInMinutes"
                :image="r.image"
                :aggregate-likes="r.aggregateLikes"
                :vegetarian="r.vegetarian"
                :vegan="r.vegan"
                :gluten-free="r.glutenFree"
                :inFavorites="r.inFavorites"
                :watched="r.watched"
                :key="r.id"
        />

    </div>

    <RecipePreviewList title="Searched Recipes result" :recipes="recipes" />



  </div>
</template>

<script>
   //import RecipePreview from "../components/RecipePreview";
   import RecipePreviewList from "../components/RecipePreviewList";

   console.log("in 1");
    export default {
        components: {
           // RecipePreview,
            RecipePreviewList         
        },
        data() {
            return {
                name: "Search",
                showDismissibleAlert:false,
                selectedCuisine: null,
                optionsCuisine: [
                    { value: null, text: 'Please select a cuisine' },
                    { value: 'African', text: 'African' },
                    { value: 'American', text: 'American' },
                    { value: 'British', text: 'British' },
                    { value: 'Cajun', text: 'Cajun' },
                    { value: 'Caribbean', text: 'Caribbean' },
                    { value: 'Chinese', text: 'Chinese' },
                    { value: 'Eastern European', text: 'Eastern European' },
                    { value: 'European', text: 'European' },
                    { value: 'French', text: 'French' },
                    { value: 'German', text: 'German' },
                    { value: 'Greek', text: 'Greek' },
                    { value: 'Indian', text: 'Indian' },
                    { value: 'Irish', text: 'Irish' },
                    { value: 'Italian', text: 'Italian' },
                    { value: 'Japanese', text: 'Japanese' },
                    { value: 'Jewish', text: 'Jewish' },
                    { value: 'Korean', text: 'Korean' },
                    { value: 'Latin American', text: 'Latin American' },
                    { value: 'Mediterranean', text: 'Mediterranean' },
                    { value: 'Mexican', text: 'Mexican' },
                    { value: 'Middle Eastern', text: 'Middle Eastern' },
                    { value: 'Nordic', text: 'Nordic' },
                    { value: 'Southern', text: 'Southern' },
                    { value: 'Spanish', text: 'Spanish' },
                    { value: 'Thai', text: 'Thai' },
                    { value: 'Vietnamese', text: 'Vietnamese' },
                ],
                selectedDiet: null,
                optionsDiet: [
                    { value: null, text: 'Please select a diet' },
                    { value: 'Gluten Free', text: 'Gluten Free' },
                    { value: 'Ketogenic', text: 'Ketogenic' },
                    { value: 'Vegetarian', text: 'Vegetarian' },
                    { value: 'Lacto-Vegetarian', text: 'Lacto-Vegetarian' },
                    { value: 'Ovo-Vegetarian', text: 'Ovo-Vegetarian' },
                    { value: 'Vegan', text: 'Vegan' },
                    { value: 'Pescetarian', text: 'Pescetarian' },
                    { value: 'Paleo', text: 'Paleo' },
                    { value: 'Primal', text: 'Primal' },
                    { value: 'Whole30', text: 'Whole30' },
                ],
                selectedIntolerance: null,
                optionsIntolerance: [
                    { value: null, text: 'Please select an intolerance' },
                    { value: 'Dairy', text: 'Dairy' },
                    { value: 'Egg', text: 'Egg' },
                    { value: 'Gluten', text: 'Gluten' },
                    { value: 'Grain', text: 'Grain' },
                    { value: 'Peanut', text: 'Peanut' },
                    { value: 'Seafood', text: 'Seafood' },
                    { value: 'Sesame', text: 'Sesame' },
                    { value: 'Shellfish', text: 'Shellfish' },
                    { value: 'Soy', text: 'Soy' },
                    { value: 'Sulfite', text: 'Sulfite' },
                    { value: 'Tree nut', text: 'Tree nut' },
                    { value: 'Wheat', text: 'Wheat' },
                ],
                selectedNum: '5',
                optionsNum: [
                    {  value: '5' ,text: '5' },
                    {  value: '10' ,text: '10' },
                    {  value: '15' , text: '15'}
                ],
                recipes: [],
                searchQuery:"",
                recipesLoaded:false,
                clickedSearch:false
            };
        },
        mounted(){
            //this.updateLastSearch();
             this.search();
            console.log("in 2");
        },
        methods:{
            async search(){
                console.log("in 3");
                this.clickedSearch=true;
                let query={};
                
                if(this.searchQuery!="") query.query=this.searchQuery;
                //let cuisine="";
                console.log(this.selectedCuisine);
                if(this.selectedCuisine!=null) query.cuisine=this.selectedCuisine.value;
                //let diet ="";
                if(this.selectedDiet!=null) query.diet=this.selectedDiet.value;
                //let intolerances = "";
                if(this.selectedIntolerance!=null) query.intolerances=this.selectedIntolerance.value;
                //let number = 5;
                query.number=this.selectedNum;
                
                console.log("query.number"+ this.selectedNum);
                const response = await this.axios.get(
                    this.$root.store.base_url + "/recipes/search", 
                    {
                        params: {query: query.query, cuisine: query.cuisine, diet: query.diet, intolerances: query.intolerance,number: this.selectedNum },                       
                    }
                );
                
                console.log(response.data.data);
                if(response.data==null){
                    console.log("in 5");
                    this.showDismissibleAlert=true;
                    this.recipes=[];
                }
                
                else{

                    console.log("in !!!!");
                    console.log(response.data.data);
                    this.showDismissibleAlert=false;
                    //const recipes = response.data;
                    //this.recipes.push(...recipes);
                    const recipes = response.data.data;
                     this.recipes.push(...recipes);
                    console.log(recipes);
                }
               // if(this.$root.store.username!="")
                 //   this.$store.lastSearch=this.recipes;

                this.recipesLoaded=true;
            },
            /*
            sortByMakingTime(){
                this.recipes.sort((a,b)=>{
                    return a.readyInMinutes-b.readyInMinutes;
                })
            },
            sortByPopularity(){
                this.recipes.sort((a,b)=>{
                    return b.aggregateLikes-a.aggregateLikes;
                })
            },
            updateLastSearch(){
                if(this.$Store.username!="")
                {
                    this.recipes=this.$store.lastSearch;
                    this.recipesLoaded=true;
                }
            }
            */
        
    
    }
    };
</script>

<style></style>