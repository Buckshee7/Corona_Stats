<template>
    <div>
        <h2>Region Select</h2>
        <div v-if="countries">
            <label>Search for a country: </label>
            <input list="options" v-model="searchValue" v-on:change="selectCountry()" />
            <datalist id="options">
                <option v-for="(country, index) in filteredCountries" :key="index" :country="country">{{ country }}</option>
            </datalist>
            <button v-if="selectedCountry" v-on:click="returnToWorld">Return to World View</button>
        </div>
        <div v-if="!countries">
            <p>Thinking about it...</p>
        </div>
    </div>
</template>

<script>
import { eventBus } from '../main.js';

export default {
    name: 'region-select',
    data(){
        return {
            searchValue: ""
        }
    },
    props: ['countries', 'selectedCountry'],
    computed: {
        filteredCountries: function(){
            return this.countries.filter((country)=>{
                return country.toLowerCase().includes(this.searchValue.toLowerCase());
            })
        }
    },
    methods: {
        selectCountry(){
            // create event only if input is a listed country
            if (this.countries.some((country) => country.toLowerCase() === this.searchValue.toLowerCase())) {
                eventBus.$emit("select-country", this.searchValue);
            }
        },
        returnToWorld(){
            this.searchValue = ""
            eventBus.$emit('return-to-world')
        }
    }
}
</script>

<style>

</style>