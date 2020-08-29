<template>
  <div>
    <header-title></header-title>
    <region-select :countries="countries"></region-select>
    <display-results></display-results>
  </div>
</template>

<script>
import HeaderTitle from '@/components/HeaderTitle.vue'
import RegionSelect from '@/components/RegionSelect.vue'
import DisplayResults from '@/components/DisplayResults.vue'
import { eventBus } from './main.js'

export default {
  name: 'app',

  mounted(){
    this.fetchWorldData();

    eventBus.$on('select-country', (country) => {
      this.selectedCountry = country
    })
  },

  data(){
    return{
      worldData: "",
      selectedCountry: "",
    }
  },

  computed: {
    // makes list of countries to pass into region-select
      // multiple provinces for some countries so need to avoid duplicates
    countries: function(){
      const countries = []
      if (!this.worldData) {
        return countries
      }
      this.worldData.confirmed.locations.forEach((location) => {
        if (!countries.includes(location.country)){
          countries.push(location.country)
        }
      })
      return countries
    },
    dataForDisplay: function(){
      if (this.worldData) {
        return this.selectedCountry ? this.makeObjectFromCountry(this.selectedCountry) : this.makeObjectFromWorld()
      }
      return {}
    }
  },

  methods:{
    fetchWorldData(){
      fetch('https://covid19api.herokuapp.com/')
      .then((resp)=>resp.json())
      .then((data)=>this.worldData = data)
    },
    makeObjectFromCountry(country){
      console.log("made it to country")
      return {location:"Country"} //not implemented
    },
    makeObjectFromWorld(){
      console.log("made it to world")
      return {location:"World"} //not implemented
    }
  },

  components: {
    'header-title':HeaderTitle,
    'region-select':RegionSelect,
    'display-results':DisplayResults
  }
}
</script>

<style>

</style>