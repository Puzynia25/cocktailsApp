<script setup>
import axios from "axios";
import { ref, computed } from "vue";
import { useRoute } from "vue-router";
import { COCKTAIL_BY_ID_URL } from "../constants";
import AppLayout from "../components/AppLayout.vue";

const route = useRoute();
const cocktailId = computed(() => route.path.split("/").pop());
const cocktail = ref(null);

async function getCocktail() {
  const data = await axios.get(`${COCKTAIL_BY_ID_URL}${cocktailId.value}`);
  cocktail.value = data?.data?.drinks[0];
}
getCocktail();

const ingredients = computed(() => {
  const ingredients = [];

  for (let i = 1; i <= 15; i++) {
    if(!cocktail.value[`strIngredient${i}`]) break

    const ingredient = {}
    ingredient.name = cocktail.value[`strIngredient${i}`];
    ingredient.measure = cocktail.value[`strMeasure${i}`];

    ingredients.push(ingredient)
  }

  return ingredients
})

</script>

<template>
  <div
    class="wrap"
    v-if="cocktail">
    <AppLayout :imgUrl="cocktail.strDrinkThumb">
      <div class="wrapper">
        <div class="info">
          <div class="title">{{ cocktail.strDrink }}</div>
          <div class="line"></div>
          <ul>
            <li>{{}}</li>
          </ul>
        </div>
      </div>
    </AppLayout>
  </div>
</template>

<style lang="sass" scoped>
@import '../assets/styles/main'
</style>
