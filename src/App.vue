<template>
  <div id="app">
    <credit-card />
  </div>
</template>

<script>
import CreditCard from "@/components/CreditCard.vue";
export default {
  name: "App",
  components: {
    CreditCard,
  },
};
</script>

<style lang="sass">
*,
*::before,
*::after
  margin: 0
  padding: 0
  box-sizing: border-box

.img-contain
  width: 100%
  height: 100%
  object-fit: contain

body
  background-color: #142125

#app
  display: grid
  place-items: center
  height: 100vh
  font-family: sans-serif

  .credit-card
    box-shadow: 0 1rem 4rem rgba(#cda66c, .5)
    background-image: linear-gradient(331deg, #41c2c7, #cda66c)
</style>
