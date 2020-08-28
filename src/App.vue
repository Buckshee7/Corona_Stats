<template>
  <div>
    <header-title></header-title>
    <region-select></region-select>
    <display-results></display-results>
  </div>
</template>

<script>
import HeaderTitle from '@/components/HeaderTitle.vue'
import RegionSelect from '@/components/RegionSelect.vue'
import DisplayResults from '@/components/DisplayResults.vue'

export default {
  name: 'app',
  data(){
    return{
      worldData: ""
  }},
  components: {
    'header-title':HeaderTitle,
    'region-select':RegionSelect,
    'display-results':DisplayResults
  },
  mounted(){
    this.fetchWorldData();
  },
  methods:{
    fetchWorldData(){
      fetch('https://api.apify.com/v2/key-value-stores/tVaYRsPHLjNdNBu7S/records/LATEST?disableRedirect=true')
      .then((resp)=>resp.json())
      .then((data)=>this.worldData = data)
    }
  }
}
</script>

<style>

</style>