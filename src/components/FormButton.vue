<template functional>
  <button>
    <slot />
  </button>
</template>

<script>
export default {

}
</script>

<style lang="sass" scoped>
  button
    padding: .5rem 1rem
    cursor: pointer
    outline: none
    border: 2px solid #555
    border-radius: .5rem
    line-height: 1
    font-size: 1rem
    text-transform: uppercase
</style>