<script setup>
import axios from "axios";
import { ref, computed } from "vue";
import { useRoute, useRouter } from "vue-router";
import { COCKTAIL_BY_ID_URL } from "../constants";
import AppLayout from "../components/AppLayout.vue";

const route = useRoute();
const router = useRouter();

const cocktailId = computed(() => route.path.split("/").pop());
const cocktail = ref(null);

async function getCocktail() {
  const data = await axios.get(`${COCKTAIL_BY_ID_URL}${cocktailId.value}`);
  cocktail.value = data?.data?.drinks[0];
}
getCocktail();

function goBack() {
  router.go(-1)
}

const ingredients = computed(() => {
  const ingredientsArr = [];

  for (let i = 1; i <= 15; i++) {
    if (!cocktail.value[`strIngredient${i}`]) break;

    const ingredientObj = {};
    ingredientObj.name = cocktail.value[`strIngredient${i}`];
    ingredientObj.measure = cocktail.value[`strMeasure${i}`];

    ingredientsArr.push(ingredientObj);
  }
  return ingredientsArr;
});

</script>

<template>
  <div
    class="wrap"
    v-if="cocktail">
    <AppLayout :imgUrl="cocktail.strDrinkThumb" :backFunc="goBack">
      <div class="wrapper">
        <div class="info">
          <div class="title">{{ cocktail.strDrink }}</div>
          <div class="line"></div>
          <div class="list">
            <div
              v-for="(ingredient, index) in ingredients"
              :key="index"
              class="list-item">
              {{ ingredient.name }}
              <template v-if="ingredient.measure">
                |
                {{ ingredient.measure }}
              </template>
            </div>
          </div>
          <div class="instructions">
            {{ cocktail.strInstructions }}
          </div>
        </div>
      </div>
    </AppLayout>
  </div>
</template>

<style lang="sass" scoped>
@import '../assets/styles/main'

li
  list-style-type: none
</style>
