<template>
  <div class="container" @click="created">
    <RecipePreview class="recipePreview"
        :recipe="this.recipe"
        :id="recipe.id"
        :title="recipe.title"
        :readyInMinutes="recipe.readyInMinutes"
        :image="recipe.image"
        :vegan="recipe.vegan"
        :vegetarian="recipe.vegetarian"
        :glutenFree="recipe.glutenFree"
        :popularity="recipe.popularity"
    ></RecipePreview>

    <div v-if="recipe">
      <div class="recipe-header mt-3 mb-4">               
        <h1>{{ recipe.title }}</h1>
        <img :src="recipe.image" class="center" />
      </div>                                            
      <div class="recipe-body">
        <div class="wrapper">
          <div class="wrapped">
            <div class="mb-3">        
              <div>Ready in {{ recipe.readyInMinutes }} minutes</div>
              <div>Likes: {{ recipe.popularity }} likes</div>
            </div>
            Ingredients:
            <ul>
              <li v-for="(r, index) in recipe.extendedIngredients"
                :key="index + '_' + r.id">
                  {{ r.original }}
              </li>
            </ul>
          </div> 
          <div class="wrapped"> 
            Instructions:
            <ol>
              <li v-for="s in recipe._instructions" :key="s.number">
                {{ s.step }}
              </li>
            </ol>
          </div>  
        </div> 
      </div> 
      <pre>
        {{ $route.params }}
        {{ recipe }}
      </pre>
    </div>

    
  </div> 
</template>

<script>
import RecipePreview from '../components/RecipePreview.vue';
export default {
    data() {
        return {
            recipe: ""
        };
    },
    async created() {
        try {
            let response;
            // response = this.$route.params.response;
            try {
                response = await this.axios.get("http://127.0.0.1:8080/recipes/" + this.$route.params.recipeId, {
                //params: { id: this.$route.params.recipeId }
                });
                // console.log("response.status", response.status);
                if (response.status !== 200)
                    this.$router.replace("/NotFound");
            }
            catch (error) {
                console.log("error.response.status", error.response.status);
                this.$router.replace("/NotFound");
                return;
            }
            console.log(response.data);
            let { analyzedInstructions, instructions, extendedIngredients, popularity, readyInMinutes, image, title } = response.data;
            let _instructions = analyzedInstructions
                .map((fstep) => {
                fstep.steps[0].step = fstep.name + fstep.steps[0].step;
                return fstep.steps;
            })
                .reduce((a, b) => [...a, ...b], []);
            let _recipe = {
                instructions,
                _instructions,
                analyzedInstructions,
                extendedIngredients,
                popularity,
                readyInMinutes,
                image,
                title
            };
            this.recipe = _recipe;
        }
        catch (error) {
            console.log(error);
        }
    },
    components: { RecipePreview }
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
</style>
