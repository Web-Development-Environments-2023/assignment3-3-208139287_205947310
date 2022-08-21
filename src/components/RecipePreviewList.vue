<template>
  <b-container>
    <b-button id="btn" v-if="showNewRandomRecipes" @click="updateRecipes" >New Random Recipes</b-button>
    <h3>
      {{ title }}:
      <slot></slot>
    </h3>
    <b-row>
      <b-col v-for="r in recipes" :key="r.id">
        <RecipePreview class="recipePreview"
        :recipe="r"
        :id="r.id"
        :title="r.title"
        :readyInMinutes="r.readyInMinutes"
        :image="r.image"
        :vegan="r.vegan"
        :vegetarian="r.vegetarian"
        :glutenFree="r.glutenFree"
        :popularity="r.popularity"
         />
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
import RecipePreview from "./RecipePreview.vue";

export default {
  name: "RecipePreviewList",
  components: {
    RecipePreview
  },
  props: {
    title: {
      type: String,
      required: true
    },
    showNewRandomRecipes: {
      type: Boolean,
      required: false,
      default: false
    }
  },
  data() {
    return {
      recipes: []
    };
  },
  mounted() {
    this.updateRecipes();
  },
  methods: {
    async updateRecipes() {
      try {
        //http://127.0.0.1:8080/recipes/random
        const response = await this.axios.get(
          "http://127.0.0.1:3000/recipes/random"
        );

        const recipes = response.data;
        this.recipes = [];
        this.recipes.push(...recipes);
        console.log(this.recipes);
      } catch (error) {
        //console.log(error);
      }
    }
  }
};
</script>

<style lang="scss" scoped>
#btn {
  
  color:black;
  width: 200px;
  background-color:lightyellow;
  border-color:forestgreen;
  border-width: 4px;
  font-weight:bolder;
}
.container {
  min-height: 400px;
}
</style>
