<template>
    <div>
        <h2>Information</h2>
        <daily-summary :dataForDisplay="dataForDisplay"></daily-summary>
        <historical-summary :dataForDisplay="dataForDisplay"></historical-summary>
    </div>
</template>

<script>
import DailySummary from './DailySummary.vue'
import HistoricalSummary from './HistoricalSummary.vue'


export default {
    name: 'display-results',
    props: ['dataForDisplay'],
    components: {
        'daily-summary': DailySummary,
        'historical-summary': HistoricalSummary
    }
}
</script>

<style>

</style>