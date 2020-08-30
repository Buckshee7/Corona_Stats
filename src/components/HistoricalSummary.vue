<template>
    <div>
        <h3>Historical Statistics</h3>
        <div>
            <h4>Filter by Dates</h4>
            <label for="from-date">From: </label>
            <input name="from-date" type="date" min="2020-01-20" :max="endDate" v-model="startDate">
            <label for="to-date">To :</label>
            <input name="to-date" type="date" :min="startDate" :max="today" v-model="endDate">
            <button v-on:click="filterByDate()">Filter</button>
            <button v-on:click="resetFilter()">Reset</button>
        </div>
        <div>
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

    </div>
</template>

<script>
import { GChart } from 'vue-google-charts'
import moment from 'moment'
import { eventBus } from '../main.js'

export default {
    name: 'historical-results',
    computed:{
        today(){
            return moment().format('YYYY[-]MM[-]DD')
        }
    },
    methods:{
        filterByDate(){
            eventBus.$emit('filter-by-date', {start:moment(this.startDate), end: moment(this.endDate)})
        },
        resetFilter(){
            eventBus.$emit('filter-by-date', {start: moment('2020-01-20'), end: moment()})
        }
    },
    data(){
        return {
            startDate: "2020-01-20",
            endDate: moment().format('YYYY[-]MM[-]DD'),
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