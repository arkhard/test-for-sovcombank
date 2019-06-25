<template lang="pug">
    form.Search(
      id="search"
      @submit="checkForm"
      method=""
    )
      .Search-top
        .Search-top-firstLine
          select-list.MainSearch-select.sss(v-show="mainSearch" :list="arrayCity" @selectItem="changeCity")
          select-list.MainSearch-select(v-show="mainSearch" :list="arrayAct" @selectItem="changeAct")
          select-list.MainSearch-select(v-show="mainSearch" :list="arrayPlace" @selectItem="changePlace")
          input.Search-top-text(v-show="!mainSearch" type="text" v-model="searchText"  placeholder="Введите название объекта (бизнес-центра, торгового центра, новостройки, логопарк)")
          input.Search-top-button(
            type="button"
            value="Найти"
            @click="checkForm"
          )
        .Search-params(v-show="mainSearch")
          .Search-params-left
            cost.Search-params-cost(v-show="showCost" @setCost="setCost")
            .button(@click="addCost" v-show="!showCost") Добавить цену
          .Search-params-right
            footage.Search-params-footage(v-show="showFootage" @setFootage="setFootage")
            .button(@click="addFootage" v-show="!showFootage") Добавить метраж
      .Search-bottom
        .Search-bottom-left
          span(:class="{ 'gray-text': !mainSearch }") Основной поиск
          label.switch
            input(type="checkbox" v-model="mainSearch")
            span.slider.round
          span(:class="{ 'gray-text': mainSearch }") Искать по названию
        .Search-bottom-right(v-show="mainSearch")
          .Search-bottom-right-button(@click="addCost")
            .circle(:class="{'rotate': showCost}")
              span +
            span Цена
          .Search-bottom-right-button(@click="addFootage")
            .circle(:class="{'rotate': showFootage}")
              span +
            span Метраж
</template>

<script>
  import MainSearch from './MainSearch';
  import Cost from './Cost';
  import Footage from './Footage';
  import SelectList from '../helpers/Select';

	export default {
		name: "index",
    data () {
		  return {

        //Переменная для проверки способа поиска
		    mainSearch: true,

        //Переменная для добавления стоимости
        showCost: false,

        //Переменная для добавления размеров
        showFootage: false,

        //Объект со стоимостью
        cost: {from: null, to: null},

        //Объект с размерами
        footage: {from: null, to: null},

        //Переменная для поиска по названию
        searchText: '',

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
        ],
      }
    },

    computed: {
      //Основной поиск
      search () {
        return {
          city: this.city,
          action: this.action,
          place: this.place,
          params: {}
        }
      },
      city: {
        get () {
          return this.arrayCity[0].value
        },
        set (newVal) {
          this.search.city = newVal.value;
        }
      },
      action: {
        get () {
          return this.arrayAct[0].value
        },
        set (newVal) {
          this.search.action = newVal.value;
        }
      },
      place: {
        get () {
          return this.arrayPlace[0].value
        },
        set (newVal) {
          this.search.place = newVal.value;
        }
      },
    },

    methods: {
      setCost (item) {
        this.cost = item;
      },
      setFootage (item) {
        this.footage = item;
      },
      addCost () {
        this.showCost = !this.showCost;
      },
      addFootage () {
        this.showFootage = !this.showFootage;
      },

      //Метод запроса поиска
      checkForm (e) {

        //Проверка, какой из поисков применить
        if(!this.mainSearch){
          if(this.searchText){
            console.log(this.searchText);
          } else {
            alert('Поле пустое');
          }
        } else {

          var error = '';

          //Проверка стоимости
          if (this.showCost){
            if (this.cost.from || this.cost.to) {
              this.search.params.cost = this.cost;
            } else {
              error += ' "стоимость" '
            }
          } else {
            this.search.params.cost = null;
          }

          //Проверка размеров
          if (this.showFootage) {
            if (this.footage.from || this.footage.to) {
              this.search.params.footage = this.footage;
            } else {
              error += ' "размер"'
            }
          } else {
            this.search.params.footage = null;
          }

          if(error){
            alert('Не введены параметры: '+error)
          } else {
            console.log(this.search)
          }

        }
      },

      changeCity (item) {
        this.city = item;
      },
      changeAct (item) {
        this.action = item;
      },
      changePlace (item) {
        this.place = item;
      }
    },
    components: {
		  MainSearch, Cost, Footage, SelectList
    }
	}
</script>

<style lang="sass" scoped>
  *:focus
    outline: none
  input
    outline: none
    &:focus
      outline: none
  .gray-text
    color: #888
  .rotate
    transform: rotate(45deg)
  .Search
    background: linear-gradient(to right, #20c1e5, #5dd8f4);
    &-top
      display: flex
      flex-flow: row wrap
      justify-content: center
      margin: auto
      padding: 30px 20px
      &-button
        width: 16%
        background: #0f9e0b
        color: white
        border: none
        border-radius: 0px 3px 3px 0px
        &:hover
          cursor: pointer
      &-firstLine
        display: flex
        width: 100%
      &-text
        width: 84%
        padding: 10px
    &-params
      display: flex
      width: 100%
      padding-top: 30px
      &-left
        display: flex
        width: 60%
        padding-right: 20px
      &-right
        display: flex
        width: 40%
        padding-left: 20px
    &-bottom
      display: flex
      background: #afe1ed
      padding: 7px
      &-left
        display: flex
        align-items: center
        width: 50%
        padding-left: 10px
      &-right
        display: flex
        justify-content: flex-end
        width: 50%
        padding-right: 7px
        &-button
          display: flex
          padding: 0px 10px
          &:hover
            cursor: pointer
          .circle
            width: 15px
            height: 15px
            background: white
            border: black 1px solid
            border-radius: 50%
            margin-right: 5px
  .button
    border: solid 1px #555
    border-radius: 3px
    padding: 10px 0px
    width: 100%
    color: #555
    &:hover
      cursor: pointer
  .MainSearch-select
    width: 28%


  @media screen and (max-width: 1800px)
    .Search
      width: 60%

  @media screen and (max-width: 1300px)
    .Search
      width: 80%

  @media screen and (max-width: 1000px)
    .Search
      width: 100%

  @media screen and (max-width: 400px)
    .MainSearch-select
      width: 100%
    .Search
      &-top
        &-firstLine
          flex-flow: row wrap
        &-button
          width: 100%
          padding: 10px
        &-text
          width: 100%
      &-params
        flex-flow: row wrap
        &-left
          width: 100%
          padding-right: 0px
        &-right
          width: 100%
          padding-left: 0px
          padding-top: 10px
      &-bottom
        flex-wrap: wrap-reverse
        &-left
          width: 100%
          padding-top: 20px
        &-right
          justify-content: center
          width: 100%

  /*Переключатель */
  .switch
    position: relative
    display: inline-block
    width: 46px
    height: 22px
    margin: 0px 10px

  .switch input
    opacity: 0
    width: 0
    height: 0

  .slider
    position: absolute
    cursor: pointer
    top: 0
    left: 0
    right: 0
    bottom: 0
    background-color: #0f9e0b
    -webkit-transition: .4s
    transition: .4s

  .slider:before
    position: absolute
    content: ""
    height: 14px
    width: 14px
    left: 3px
    bottom: 4px
    background-color: white
    -webkit-transition: .4s
    transition: .4s

  input:checked + .slider:before
    -webkit-transform: translateX(26px)
    -ms-transform: translateX(26px)
    transform: translateX(26px)

  .slider.round
    border-radius: 34px


  .slider.round:before
    border-radius: 50%
</style>
