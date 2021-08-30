<template>
  <div id="app">
    <FilterApp :list_data = 'getBranchesAndRegions' />
  </div>
</template>

<script>
import FilterApp from './components/Filter.vue'

export default {
  name: 'App',
  components: {
    FilterApp
  },
  data() {
    return {
      getBranchesAndRegions: {
        CN: {
          branchCode: 'CN', //код филиала
          branchName: 'Центральный филиал',//Имя филиала
          regions: [ //список регионов внутри филиала
            {name: 'Брянская область', code: 32}, //name: имя региона, code: код региона
            {name: 'Владимирская область', code: 33},
            {name: 'Калужская область', code: 40},
            {name: 'Курская область', code: 46},
          ]
        },
        KV: {
          branchCode: 'KV',
          branchName: 'Кавказский филиал',
          regions: [
            {name: 'Республика Адыгея', code: 1},
            {name: 'Республика Дагестан', code: 5},
            {name: 'Краснодарский край', code: 23},
            {name: 'Воронежская область', code: 36},
          ]
        }
      }
    }
  },
}


</script>

<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>
