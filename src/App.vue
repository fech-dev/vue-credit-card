<template>
  <div id="app">
    <form>
      <credit-card v-model="order.creditCard"/>
      <form-button> 👉 Invia dati </form-button>
    </form>
  </div>
</template>

<script>

import CreditCard from "@/components/CreditCard.vue";
import FormButton from '@/components/FormButton.vue';
export default {
  name: "App",
  components: {
    CreditCard,
    FormButton,
  },

  data(){
    return{
      creditCardsInfo: null,
      order: {
        creditCard: {}
      }
    }
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

  form
    button
      margin-top: 1rem
</style>
