<template>
  <div class="credit-card">
    <div class="credit-card__holder-name">
      <form-input
        type="text"
        label="Name"
        placeholder="John"
        v-model="cardData.name"
      />
    </div>

    <div class="credit-card__holder-surname">
      <form-input
        type="text"
        label="Surname"
        placeholder="Doe"
        v-model="cardData.surname"
      />
    </div>

    <div class="credit-card__number">
      <form-input
        type="text"
        label="Number"
        placeholder="#### #### #### ####"
        v-mask="'#### #### #### ####'"
        v-model="cardData.number"
      />
    </div>

    <div class="credit-card__expiration">
      <form-input
        type="text"
        label="Expiration Date"
        placeholder="MM/YY"
        v-mask="'##/##'"
        v-model="cardData.expirationDate"
      />
    </div>

    <div class="credit-card__cvv">
      <form-input
        type="password"
        label="Cvv"
        placeholder="123"
        v-model="cardData.cvv"
      />
    </div>

    <div class="credit-card__type-logo" v-if="cardInfo.logo">
      <img class="img-contain" :src="cardInfo.logo" :alt="`cardInfo.name`" />
    </div>
  </div>
</template>

<script>
import axios from "axios";
import FormInput from "@/components/FormInput.vue";
export default {
  name: "CreditCard",
  components: { FormInput },
  data() {
    return {
      cardsInfo: [],
      cardData: {
        name: "",
        surname: "",
        number: "",
        expirationDate: "",
        cvv: "",
      },
    };
  },
  computed: {
    cardInfo() {
      return (
        this.cardsInfo.find(({ iins }) =>
          iins.some((iin) =>
            this.cardData.number.toString().match(iin.toString())
          )
        ) || {}
      );
    },
  },
  methods: {
    async loadCardInfo() {
      const { data } = await axios.get("/cards-data.json");
      this.cardsInfo = data;
    },
  },
  created() {
    this.loadCardInfo();
  },
};
</script>

<style lang="sass">
.credit-card
  display: grid
  grid-template-columns: repeat(3, 1fr) 4rem
  grid-template-areas: "name name surname surname" "number number number number" "expiration expiration cvv logo"
  gap: 2.5rem 1rem
  width: 30rem
  padding: 1rem 1.5rem
  border-radius: .6rem
  color: #fff
  background-image: linear-gradient(to bottom right, #333, #535353)
  font-size: 1.5rem

  &__holder-name
    grid-area: name

  &__holder-surname
    grid-area: surname

  &__type-logo
    grid-area: logo
    background-color: #fff
    padding: 4px

  &__number
    grid-area: number

  &__expiration
    grid-area: expiration

  &__cvv
    grid-area: cvv
</style>