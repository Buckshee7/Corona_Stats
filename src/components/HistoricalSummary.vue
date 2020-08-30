<template>
    <div>
        <h3>Historical Statistics</h3>
        <GChart
            type="LineChart"
            :data="dataForDisplay.historicalTotals"
            :options="chartOptions1"
        />
        <GChart
            type="LineChart"
            :data="dataForDisplay.historicalChange"
            :options="chartOptions2"
        />
    </div>
</template>

<script>
import { GChart } from 'vue-google-charts'

export default {
    name: 'historical-results',
    data(){
        return {
            chartOptions1: {
                title: 'Total Cases, Deaths, and Recoveries by Date',
                hAxis: {
                    title: 'Date',
                },
                vAxis: {
                    title: 'Total Number',
                }
             },
            chartOptions2: {
                title: 'Daily Change in Cases, Deaths, and Recoveries by Date',
                hAxis: {
                    title: 'Date',
                },
                vAxis: {
                    title: 'Daily Change',
                }
             }
        }
    },
    props: ['dataForDisplay'],
    components: {
        GChart
    }
}
</script>

<style>

</style>