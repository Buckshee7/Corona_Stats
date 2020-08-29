<template>
  <div>
    <header-title></header-title>
    <region-select :countries="countries"></region-select>
    <display-results :dataForDisplay="dataForDisplay"></display-results>
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

    getDateArray(startDate, endDate, format){
      const dates = []
      const days = endDate.diff(startDate, 'days')
      for (let date = startDate.clone(); date < endDate; date=date.add(1, 'day')){
        let newEntry = date.format(format)
        dates.push([newEntry])
      }
      return dates
    },

    makeObjectFromCountry(country){
      const returnData = {location:country, latest:{confirmed:null, deaths:null, recovered:null}, historical:[]}

      //for each day since records began (22/01/20) create an array with the date as the first item
      const dates = this.getDateArray(moment('2020-01-20'), moment().subtract(1, 'day'), 'M[/]D[/]YY')

      for (let key in returnData.latest) {
      //get latest
        //get locations that match country
        const countryLocations = this.worldData[key].locations.filter((location)=>{return location.country.toLowerCase() === country.toLowerCase()})
        //get total of all locations
        const total = countryLocations.reduce((accumulator, location)=>{return accumulator + location.latest},0)
        //set to latest data
        returnData.latest[key] = total

      //get historical data for charts
        for (let date of dates){
          //total the values from countryLocations
          let total = countryLocations.reduce((accumulator,location)=>{
            //from the object with matching date if it exists
            return accumulator + location.history[date[0]] ? location.history[date[0]] : 0
          },0)
          // I'll probably get told off here for modifying an array im looping through...........
            // .....but its an array **within** the array im looping through so maybe thats not so bad
          date.push(total)
        }
      }

      //add column headers to dates
      dates.unshift(["Date", "Confirmed Cases", "Deaths", "Recovered"])
      returnData.historical = dates

      return returnData
    },

    makeObjectFromWorld(){
      const returnData = {location:"World", latest:{confirmed:null, deaths:null, recovered:null}, historical:[]}

      //create list with object for every country
      const countryObjects = []
      for (let country of this.countries){
        countryObjects.push(this.makeObjectFromCountry(country))
      }

      //sum up the values from every country in the same data structure
      returnData.latest.confirmed = countryObjects.reduce((accumulator, countryObject) => accumulator + countryObject.latest.confirmed,0)
      returnData.latest.deaths = countryObjects.reduce((accumulator, countryObject) => accumulator + countryObject.latest.deaths,0)
      returnData.latest.recovered = countryObjects.reduce((accumulator, countryObject) => accumulator + countryObject.latest.recovered,0)

      const dates = this.getDateArray(moment('2020-01-20'), moment().subtract(1, 'day'), 'M[/]D[/]YY')
      // for each date there needs to be a result in historical
      for (let date of dates){
        let worldValues = []
        // each result need the 3 figures
        for (let i=1; i<4; i++){
          // add up the figures by going through every country and assign to temporary array
          worldValues[i-1] = countryObjects.reduce((accumulator, countryObject) => {
            return accumulator + countryObject.historical.find((dateList)=>{return dateList[0]=== date[0]})[i]
          },0)
        }
        returnData.historical.push([date[0], worldValues[0], worldValues[1], worldValues[2]])
      }
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