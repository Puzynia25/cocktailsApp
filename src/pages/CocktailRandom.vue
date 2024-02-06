<script setup>
import axios from "axios";
import { ref, computed } from "vue";
import { useRoute, useRouter } from "vue-router";
import { COCKTAIL_BY_RANDOM, INGREDIENT_PIC } from "../constants";
import AppLayout from "../components/AppLayout.vue";

import { Swiper, SwiperSlide } from "swiper/vue";
import "swiper/css";

const route = useRoute();
const router = useRouter();

const cocktail = ref(null);

async function getCocktail() {
    const data = await axios.get(COCKTAIL_BY_RANDOM);
    cocktail.value = data?.data?.drinks[0];
}
getCocktail();

function goBack() {
    router.go(-1);
}

const ingredients = computed(() => {
    const ingredientsArr = [];

    for (let i = 1; i <= 15; i++) {
        if (!cocktail.value[`strIngredient${i}`]) break;

        const ingredient = cocktail.value[`strIngredient${i}`];

        ingredientsArr.push(ingredient);
    }
    return ingredientsArr;
});
</script>

<template>
    <div
        class="wrap"
        v-if="cocktail">
        <AppLayout
            :imgUrl="cocktail.strDrinkThumb"
            :backFunc="goBack">
            <div class="wrapper">
                <div class="info">
                    <div class="title">{{ cocktail.strDrink }}</div>
                    <div class="line"></div>
                    <div class="slider">
                        <swiper
                            :slides-per-view="3"
                            :space-between="50"
                            class="swiper">
                            <swiper-slide
                                v-for="(ingredient, key) in ingredients"
                                :key="key">
                                <img
                                    class="img"
                                    :src="`${INGREDIENT_PIC}${ingredient}-Small.png`"
                                    :alt="cocktail.strDrinkThumb" />
                                <div class="name">{{ ingredient }}</div>
                            </swiper-slide>
                        </swiper>
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

.slider
    padding: 50px 0
.swiper
    display: flex
    justify-content: justify-between
    width: 586px

.img
    width: 100px
    height: 100px

.name
    padding-top: 20px
</style>
