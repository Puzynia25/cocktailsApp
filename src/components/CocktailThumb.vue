<script setup>
import { RouterLink } from "vue-router";

const props = defineProps({
  cocktail: {
    type: Object,
    required: true,
  },
});
</script>

<template>
  <div class="root" >
    <RouterLink :to="`/cocktails/${cocktail.idDrink}`">
      <div
        :style="`background-image: url(${cocktail.strDrinkThumb}) `"
        class="pic"></div>
    </RouterLink>
    <span class="name">{{ cocktail.strDrink }}</span>
  </div>
</template>

<style lang="sass" scoped>
@import '../assets/styles/main'

.root
  width: calc( (100% - 76px) / 3 )
  padding-bottom: 20px

.pic
  height: 120px
  width: 120px
  background-repeat: no-repeat
  background-position: 50% 50%
  background-size: 100%
  margin: 0 auto

  &:hover
    filter: drop-shadow(0.5rem 0.5rem 0.75rem)

.name
  display: block
  margin-top: 15px
  width: 100%
  white-space: nowrap
  overflow: hidden
  text-overflow: ellipsis
  letter-spacing: 0.1em
</style>
