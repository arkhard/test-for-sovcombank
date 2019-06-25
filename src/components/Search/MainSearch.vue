<template lang="pug">
  .MainSearch
    p {{selected}}
    select-list.MainSearch-select(:list="arrayCity" @selectItem="changeCity")
    select-list.MainSearch-select(:list="arrayAct" @selectItem="changeAct")
    select-list.MainSearch-select(:list="arrayPlace" @selectItem="changePlace")
</template>

<script>
  import SelectList from '../helpers/Select';

	export default {
		name: "MainSearch",
    data () {
		  return {
        city: null,
        action: null,
        place: null,
        arrayCity: [
          {name: 'Москва', value: 'Москва', id: 1},
          {name: 'Санкт-Петербург', value: 'Санкт-Петербург', id: 2},
        ],
        arrayAct: [
          {name: 'Купить', value: 'buy', action: 'buy'},
          {name: 'Арендовать', value: 'arend', action: 'arend'},
        ],
        arrayPlace: [
          {name: 'Офис', value: 'office', place: 'office'},
          {name: 'Квартира', value: 'apartment', place: 'apartment'},
        ]
      }
    },
    computed: {
		  selected: {
		    get: function () {
		      let item = {city: this.arrayCity[0].value, action: this.arrayAct[0].value, place: this.arrayPlace[0].value}
          this.$emit('change', item);
		      return item
        },
        set: function (newVal) {
		      console.log(this.selected)
          this.$emit('change', newVal)
        }
      }
    },
    methods: {
		  changeCity (item) {
		    this.selected.city = item.value;
      },
      changeAct (item) {
        this.selected.action = item.value;
      },
      changePlace (item) {
        this.selected.place = item.value;
      }
    },
    components: {
		  SelectList
    }
	}
</script>

<style lang="sass" scoped>
  .MainSearch
    display: flex
    &-select
      flex-grow: 1
</style>
