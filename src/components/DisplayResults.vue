<template>
    <div>
        <h2>Information</h2>
        <daily-summary></daily-summary>
        <historical-summary></historical-summary>
    </div>
</template>

<script>
import DailySummary from './DailySummary.vue'
import HistoricalSummary from './HistoricalSummary.vue'


export default {
    name: 'display-results',
    components: {
        'daily-summary': DailySummary,
        'historical-summary': HistoricalSummary
    }
}
</script>

<style>

</style>