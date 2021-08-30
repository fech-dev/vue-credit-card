<template>
  <div class="form-control">
    <input
      v-bind="$attrs"
      :value="value"
      @input="$emit('input', $event.target.value)"
    />
    <label v-if="label" :for="$attrs.id">{{ label }}</label>
  </div>
</template>

<script>
export default {
  inheritAttrs: false,
  props: {
    value: {},
    label: {
      type: String,
    },
  },
};
</script>

<style lang="sass">
.form-control
  display: flex
  flex-direction: column

  label
    font-size: 1rem
    text-transform: uppercase

  input
    width: 100%
    background-color: transparent
    border: none
    border-bottom: 2px solid #fff
    padding: .25rem .325rem
    font-size: 1.125rem
    outline: none
    color: inherit
    transition: all .3s ease

    &:focus
      background-color: rgba(#fff, .125)

</style>