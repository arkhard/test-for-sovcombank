<template lang="pug">
  .Select
    select.s(v-bind="selected" @change="change" :style="setStyle")
      option(v-for="(item, index) in array" :key="index" :value="item.value") {{item.name}}
</template>

<script>
	export default {
		name: "Select",
    props: ['list', 'setStyle'],
    computed: {
		  array () {
		    if (this.list)
		      return this.list
        return null
      },
      selected () {
        if (this.array)
          return this.array[0]
        return {value: 'Список пуст'}
      }
    },
    methods: {
		  change (e) {
		    let item = this.array.find( (element)=> {
		      return element.value === e.target.value
        })
		    this.$emit('selectItem', item)
      }
    },
	}
</script>

<style lang="sass" scoped>
  .Select
    select
      width: 100%
      padding: 10px 10px
      background: white
      border: lightblue 1px solid
</style>
