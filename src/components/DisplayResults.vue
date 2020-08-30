<template>
    <div>
        <h2>Information</h2>
        <div v-if="dataForDisplay">
            <daily-summary  :dataForDisplay="dataForDisplay"></daily-summary>
            <historical-summary :dataForDisplay="dataForDisplay"></historical-summary>
        </div>
        <div v-if="!dataForDisplay">
            <p>Thinking about it...</p>
        </div>
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