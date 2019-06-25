<template lang="pug">
  .Cost
    input.leftCircle( type="text" v-model="from" placeholder="От" )
    input( type="text" v-model="to" placeholder="До" )
    select-list(:list="arrayCurrency" @selectItem="changeCurrency")
</template>

<script>
  import SelectList from '../helpers/Select';

	export default {
		name: "Cost",
    data () {
		  return {
        arrayCurrency: [
          {name: '₽/месяц', value: 'rubles'},
          {name: '$/месяц', value: 'dollars'}
        ]
      }
    },
    computed: {
      cost () {
        return {
          from: this.from,
          to: this.to,
          currency: this.currency
        }
      },
      from: {
        get: () => null ,
        set (newVal) {
          this.cost.from = newVal;
          this.$emit('setCost', this.cost);
        }
      },
      to: {
        get: () => null ,
        set (newVal) {
          this.cost.to = newVal;
          this.$emit('setCost', this.cost);
        }
      },
      currency: {
        get () {
          return this.arrayCurrency[0].value
        },
        set (newVal) {
          this.cost.currency = newVal.value;
          this.$emit('setCost', this.cost);
        }
      },
    },
    methods: {
		  changeCurrency (item) {
		    this.currency = item;
      }
    },
    components: {
		  SelectList
    }
	}
</script>

<style lang="sass" scoped>
  .Cost
    display: flex
    width: 100%
    input
      width: 33%
      padding-left: 10px
      border: lightblue 1px solid
    .Select
      width: 33%
  .leftCircle
    border-radius: 3px 0px 0px 3px
</style>
