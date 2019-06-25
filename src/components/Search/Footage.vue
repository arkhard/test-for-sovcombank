<template lang="pug">
  .Footage
    input.leftCircle( type="text" v-model="from" placeholder="От" )
    input( type="text" v-model="to" placeholder="До" )
    .Footage-symbol.rightCircle
      span м²
</template>

<script>
  import SelectList from '../helpers/Select';

  export default {
    name: "Footage",
    data () {
      return {
        from: null,
        to: null,
      }
    },
    watch: {
      from (newVal) {
        this.$emit('setFootage', {from: newVal, to: this.to})
      },
      to (newVal) {
        this.$emit('setFootage', {from: this.from, to: newVal})
      }
    },
    components: {
      SelectList
    }
  }
</script>

<style lang="sass" scoped>
  .Footage
    display: flex
    width: 100%
    input
      width: 40%
      border: lightblue 1px solid
      padding: 10px 10px
    &-symbol
      display: flex
      width: 20%
      background: #8fe3f7
      span
        margin: auto
  .leftCircle
    border-radius: 3px 0px 0px 3px
  .rightCircle
    border-radius: 0px 3px 3px 0px
</style>
