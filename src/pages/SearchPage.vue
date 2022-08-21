<template>
  <div class="container">
    <h1 class="title">Search Page</h1>

    <div class="search-filters">
      <div>
        <span>Amount</span>
        <b-form-select v-model="searchAmount" :options="searchAmountOptions"></b-form-select>
      </div>
      <div>
        <span>Cuisine</span>
        <b-form-select v-model="searchCuisine" :options="searchCuisineOptions"></b-form-select>
      </div>
      <div>
        <span>Diet</span>
        <b-form-select v-model="searchDiet" :options="searchDietOptions"></b-form-select>
      </div>
      <div>
        <span>Intolerances</span>
        <b-form-select v-model="searchIntolerances" :options="searchIntolerancesOptions"></b-form-select>
      </div>
    </div>

    <b-input-group prepend="Search Query:" id="search-input">
      <b-form-input v-model="searchQuery"></b-form-input>
      <b-input-group-append>
        <b-button type="submit" @click="Search" variant="success">Search</b-button>

        <br>

      </b-input-group-append>

    </b-input-group>


    <h2 v-if="!searchResult.length" class="description-text">
      <span v-if="!searched"> Please search for recipes to show results </span>
      <span v-if="searched"> No Results were found </span>
    </h2>

    <div class="search-result">
      <div class="search-result-item" v-for="r in searchResult" :key="r.id">
        <RecipePreview class="recipePreview" :recipe="r"/>
      </div>

    </div>

    <div class="result_container"></div>

  </div>
</template>
<script>

  import RecipePreview from "../components/RecipePreview.vue";

  export default {
    components: {
      RecipePreview
    },
    name: "SearchPage",
    data() {
      return {
        searched: false,
        searchAmountOptions: [
          {value: "5", text: "5"},
          {value: "10", text: "10"},
          {value: "15", text: "15"},
        ],
        searchCuisineOptions: [
          {value: "", text: "Any"},
          {value: "African", text: "African"},
          {value: "American", text: "American"},
          {value: "British", text: "British"},
          {value: "Cajun", text: "Cajun"},
          {value: "Caribbean", text: "Caribbean"},
          {value: "Chinese", text: "Chinese"},
          {value: "Eastern European", text: "Eastern European"},
          {value: "European", text: "European"},
          {value: "French", text: "French"},
          {value: "German", text: "German"},
          {value: "Greek", text: "Greek"},
          {value: "Indian", text: "Indian"},
          {value: "Irish", text: "Irish"},
          {value: "Italian", text: "Italian"},
          {value: "Japanese", text: "Japanese"},
          {value: "Jewish", text: "Jewish"},
          {value: "Korean", text: "Korean"},
          {value: "Latin American", text: "Latin American"},
          {value: "Mediterranean", text: "Mediterranean"},
          {value: "Mexican", text: "Mexican"},
          {value: "Middle Eastern", text: "Middle Eastern"},
          {value: "Nordic", text: "Nordic"},
          {value: "Southern", text: "Southern"},
          {value: "Spanish", text: "Spanish"},
          {value: "Thai", text: "Thai"},
          {value: "Vietnamese", text: "Vietnamese"},
        ],
        searchDietOptions: [
          {value: "", text: "Any"},
          {value: "Gluten Free", text: "Gluten Free"},
          {value: "Ketogenic", text: "Ketogenic"},
          {value: "Vegetarian", text: "Vegetarian"},
          {value: "Lacto-Vegetarian", text: "Lacto-Vegetarian"},
          {value: "Ovo-Vegetarian", text: "Ovo-Vegetarian"},
          {value: "Vegan", text: "Vegan"},
          {value: "Pescetarian", text: "Pescetarian"},
          {value: "Paleo", text: "Paleo"},
          {value: "Primal", text: "Primal"},
          {value: "Low FODMAP", text: "Low FODMAP"},
          {value: "Whole30", text: "Whole30"},
        ],
        searchIntolerancesOptions: [
          {value: "", text: "Any"},
          {value: "Dairy", text: "Dairy"},
          {value: "Egg", text: "Egg"},
          {value: "Gluten", text: "Gluten"},
          {value: "Grain", text: "Grain"},
          {value: "Peanut", text: "Peanut"},
          {value: "Seafood", text: "Seafood"},
          {value: "Sesame", text: "Sesame"},
          {value: "Shellfish", text: "Shellfish"},
          {value: "Soy", text: "Soy"},
          {value: "Sulfite", text: "Sulfite"},
          {value: "Tree Nut", text: "Tree Nut"},
          {value: "Wheat", text: "Wheat"},
        ],
        localFild: {value: 'id', text: 'choose'},

        dropdown_offset: "dropdown-offset",
        //end trying
        searchAmount: "5",
        searchQuery: "",
        searchCuisine: "",
        searchDiet: "",
        searchIntolerances: "",
        recipe: [],
        recipeList_details: [],
        recipe_details: [],
        recipe_details_fixed: [],// the fixed name of recipe query
        searchResult: []
      };
    },
    methods: {
      async Search() {
        let recipe = [];
        let url = `http://localhost:3000/recipes/search`;
        url += `?query=${this.searchQuery}`;
        url += `&amount=${this.searchAmount}`;
        url += `&cuisine=${this.searchCuisine}`;
        url += `&diet=${this.searchDiet}`;
        url += `&intolerances=${this.searchIntolerances}`;
        recipe = await this.axios.get(url);
        this.searchResult = recipe.data;
        this.searched = true;
      }
    },
  }
</script>
<style>
  .search-result {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
    padding-top: 1em;
  }

  .search-result-item {
    max-width: 400px;
  }

  .search-filters {
    display: flex;
    padding: 10px 0;
  }

  .search-filters div{
    display: flex;
    align-content: center;
    justify-content: center;
    padding-left: 8px;
    vertical-align: center;
  }
  .search-filters span{
    align-self: center;
    padding-right: 6px;
  }

  .description-text{
    text-align: center;
    padding: 1em;
    color: #5f5f5f;
  }
</style>
