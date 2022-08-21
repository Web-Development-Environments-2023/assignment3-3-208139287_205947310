<template>
  <div class="recipe-preview">
    <router-link
       :to="{ name: 'recipe', params: { recipeId: recipe.id } }"
    >
      <div class="recipe-image">
        <img :src="recipe.image ? recipe.image : 'https://gaash-customs.co.il/wp-content/themes/elinsTemplate/assets/img/img_not_found.png'" class="recipe-image"/>
      </div>
      <div :title="recipe.title" class="recipe-title" >
        {{ recipe.title }}
      </div>
    </router-link>
    <div class="recipe-footer">
      <ul class="recipe-overview">
        <li>{{ recipe.readyInMinutes }} minutes</li>
        <li>{{ recipe.popularity }} likes</li>
      </ul>
      <ul class="recipe-overview">
        <li :style="{'color': recipe.vegan ? 'green' : 'red'}"> Vegan</li>
        <li :style="{'color': recipe.vegetarian ? 'green' : 'red'}"> Vegetarian</li>
        <li :style="{'color': recipe.glutenFree ? 'green' : 'red'}"> Gluten Free</li>
      </ul>
    </div>
  </div>
</template>

<script>
  export default {
    name: "recipe-body",
    mounted() {
      this.axios.get(this.recipe.image).then((i) => {
        this.image_load = true;
      });
    },
    data() {
      return {
        image_load: false
      };
    },
    props: {
      id: {
        type: Number,
        required: true
      },
      recipe: {
        type: Object,
        required: true
      },
      title: {
        type: String,
        required: true
      },
      readyInMinutes: {
        type: Number,
        required: true
      },
      image: {
        type: String,
        required: true
      },
      vegan: {
        type: Boolean,
        required: true
      },
      vegetarian: {
        type: Boolean,
        required: true
      },
      glutenFree: {
        type: Boolean,
        required: true
      },
      popularity: {
        type: Number,
        required: false,
        default() {
          return undefined;
        }
      }
    }
  };
</script>

<style scoped>
  .recipe-preview {
    display: flex;
    flex-direction: column;
    min-width: 340px;
    width: 340px;
    max-width: 340px;
    padding-bottom: 10px;
  }

  .recipe-preview img {
    min-width: 340px;
    width: 340px;
    max-width: 340px;
  }

  .recipe-preview .recipe-title {
    padding-top: 8px;
    color: black;
    font-size: 18px;
  }

  .recipe-overview {
    list-style: none;
    display: flex;
    justify-content: space-between;
    padding: 4px 0;
    margin: 0;
  }
</style>
