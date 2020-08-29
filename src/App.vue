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
import moment from 'moment'

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
      if (this.worldData) {
        this.worldData.confirmed.locations.forEach((location) => {
          if (!countries.includes(location.country)){
            countries.push(location.country)
          }
        })
      }
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

    getDateArray(startDate, endDate){
      const dates = []
      const days = endDate.diff(startDate, 'days')
      for (let date = startDate.clone(); date < endDate; date=date.add(1, 'day')){
        let newEntry = date.format('M[/]D[/]YY')
        dates.push([newEntry])
      }
      return dates
    },

    makeObjectFromCountry(country){
      const returnData = {location:country, latest:{confirmed:null, deaths:null, recovered:null}, historical:[]}

      ////for each day since records began (22/01/20) create an array with the date as the first item
      let dates = this.getDateArray(moment('2020-01-20'), moment())
      console.log(dates)


      for (let key in returnData.latest) {
      //get latest
        //get locations that match country
        const countryLocations = this.worldData[key].locations.filter((location)=>{return location.country.toLowerCase() === country.toLowerCase()})
        //get total of all locations
        const total = countryLocations.reduce((accumulator, location)=>{return accumulator + location.latest},0)
        //set to latest data
        returnData.latest[key] = total

      //get historical


      }

      
      //for each of confirmed,deaths,recovered; match the date and 

      return returnData
    },
    makeObjectFromWorld(){
      const returnData = {location:"World", latest:{confirmed:null, deaths:null, recovered:null}, historical:[]}
      return returnData
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