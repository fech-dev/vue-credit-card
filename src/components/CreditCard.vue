<template>
  <div class="credit-card" ref="credit-card">
    <div class="credit-card__holder-name">
      <component :is="FormInputComponent"
        type="text"
        label="Name"
        placeholder="John"
        :value="value.name"
        @input="emitValue('name', $event)"
      />
    </div>

    <div class="credit-card__holder-surname">
      <component :is="FormInputComponent"
        type="text"
        label="Surname"
        placeholder="Doe"
         :value="value.surname"
        @input="emitValue('surname', $event)"
      />
    </div>

    <div class="credit-card__number">
      <component :is="FormInputComponent"
        type="text"
        label="Number"
        placeholder="1234 1234 1234 1234"
        v-mask="cardInfo.mask"
        :value="value.number"
        @input="emitValue('number', $event)"
      />
    </div>

    <div class="credit-card__expiration">
      <component :is="FormInputComponent"
        type="text"
        label="Expiration Date"
        placeholder="MM/YY"
        v-mask="'##/##'"
        :value="value.expirationDate"
        @input="emitValue('expirationDate', $event)"
      />
    </div>

    <div class="credit-card__cvv">
      <component :is="FormInputComponent"
        type="password"
        label="Cvv"
        placeholder="123"
        :value="value.cvv"
        @input="emitValue('cvv', $event)"
      />
    </div>

    <div class="credit-card__type-logo" v-if="cardInfo.logo">
      <img class="img-contain" :src="cardInfo.logo" :alt="`cardInfo.name`" />
    </div>
  </div>
</template>

<script>
const defaultCardModel = {
  colors: ['#111111', '#535353'],
  mask: '#### #### #### ####'
}

export default {
  name: "CreditCard",
  props: {
    schemasURL: {
      type: [String, URL],
      default: '/cards-data.json'
    },
    FormInputComponent: {
      type: [String, Object],
      default: () => require("@/components/FormInput.vue").default
    },
    value: {
      type: Object,
      default: () => ({})
    }
  },
  data() {
    return {
      cardModels: [],
      cardProxy: {
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
      let info

      if(this.cardModels.length){
        info = this.cardModels.find(({ codeRegexps }) => 
          codeRegexps.some(
            codeRegexp => new RegExp(codeRegexp).test(this.cardProxy.number.replace(' ', ''))
          )
        )
      }

      return info || defaultCardModel
    },
  },
  methods: {
    emitValue(key, value){
      this.cardProxy[key] = value
      this.$emit('input', {
        ...this.cardProxy,
        info: this.cardInfo
      })
    },

    async loadCreditCardsInfo() {
      const resp = await fetch("/cards-data.json");
      this.cardModels = await resp.json();
    },
   
    setCreditCardBackground(colors){
      //override --credit-card-bg-color-from & --credit-card-bg-color-to css vars
      const $creditCardEl = this.$refs['credit-card']
      if($creditCardEl){
        $creditCardEl.style.setProperty('--credit-card-bg-color-from', colors[0])
        $creditCardEl.style.setProperty('--credit-card-bg-color-to', colors[1])
      }
    }
  },
  
  watch: {
    'cardInfo.colors'(colors){
      this.setCreditCardBackground(colors)
    }
  },
  created(){
    this.loadCreditCardsInfo()
  }
};
</script>

<style lang="sass">
.credit-card
  --credit-card-bg-color-from: #111111
  --credit-card-bg-color-to: #535353

  position: relative
  display: grid
  grid-template-columns: repeat(3, 1fr) 4rem
  grid-template-areas: "name name surname surname" "number number number number" "expiration expiration cvv logo"
  gap: 2.5rem 1rem
  width: 30rem
  padding: 1rem 1.5rem
  border-radius: .6rem
  color: #fff
  font-size: 1.5rem

  &::after
    content: ''
    display: block
    position: absolute
    top: 0
    left: 0
    right: 0
    bottom: 0
    border-radius: inherit
    background-image: linear-gradient(to bottom right, var(--credit-card-bg-color-from), var(--credit-card-bg-color-to))
    box-shadow: 0 1rem 3rem rgba(#111, .5)
    z-index: -1
    opacity: .75

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