<template>
    <div>
        <h3>Latest Statistics: {{ dataForDisplay.location }}</h3>
        <p>Total Confirmed Cases: {{ dataForDisplay.latest.confirmed }}</p>
        <p>Total Deaths: {{ dataForDisplay.latest.deaths }}</p>
        <p>Total Recoveries: {{ dataForDisplay.latest.recovered }}</p>
    </div>
</template>

<script>
export default {
    name: 'daily-summary',
    props: ['dataForDisplay']
}
</script>

<style>

</style>