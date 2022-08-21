<template>
  <div class="container">
    <h1 class="title">Main Page</h1>
    <RecipePreviewList :showNewRandomRecipes="true" title="Randome Recipes" class="RandomRecipes" center />
    
    <router-link id="btn1" v-if="!$root.store.username" to="/login" tag="b-button">You need to Login to vue this</router-link>
    <!-- {{ !$root.store.username}} -->
    <RecipePreviewList
      title="Last Viewed Recipes"
      :class="{
        RandomRecipes: true,
        blur: !$root.store.username,
        center: true
      }"
      disabled
    ></RecipePreviewList>
    <div
      style="position: absolute;top: 70%;left: 50%;transform: translate(-50%, -50%);"
    >
      <!-- Centeredasdasdad -->
    </div>
  </div>
</template>

<script>
import RecipePreviewList from "../components/RecipePreviewList";
export default {
  components: {
    RecipePreviewList,
  }
};
</script>

<style lang="scss" scoped>
#btn1 {
  color:black;
  width: 290px;
  background-color:lightyellow;
  border-color:forestgreen;
  border-width: 4.5px;
  font-weight:bolder;
}

.RandomRecipes {
  margin: 10px 0 10px;
}
.blur {
  -webkit-filter: blur(5px); /* Safari 6.0 - 9.0 */
  filter: blur(2px);
}
::v-deep .blur .recipe-preview {
  pointer-events: none;
  cursor: default;
}
</style>
