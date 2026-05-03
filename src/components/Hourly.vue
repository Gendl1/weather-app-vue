<template>
    <aside class="hero__forecast-hourly">
        <div class="hero__forecast-hourly-title">
            <div class="hero__forecast-hourly-title-name">
            Hourly forecast
            </div>

            <HourlyTitleWeek/>
            
        </div>

        <div class="hero__forecast-hourly-grid">
            <HourlyGrid
            v-for="card in HourlyGridCards"
            :icon="card.icon"
            :time="card.time"
            :temp="card.temp"
            />
        </div>
    </aside>
</template>

<script setup>
import { computed } from 'vue';
import HourlyGrid from './HourlyGrid.vue';
import HourlyTitleWeek from './HourluTitleWeek.vue'

const HourlyGridCards = computed(() => [
    {
        icon: '/images/icon-overcast.webp',
        time: '0 AM',
        temp: '20°'
    },
    {
        icon: '/images/icon-partly-cloudy.webp',
        time: '2 AM',
        temp: '20°'
    },
    {
        icon: '/images/icon-sunny.webp',
        time: '4 AM',
        temp: '20°'
    },
    {
        icon: '/images/icon-partly-cloudy.webp',
        time: '6 AM',
        temp: '18°'
    },
    {
        icon: '/images/icon-snow.webp',
        time: '8 AM',
        temp: '15°'
    },
    {
        icon: '/images/icon-fog.webp',
        time: '10 AM',
        temp: '20°'
    },
    {
        icon: '/images/icon-snow.webp',
        time: '12 AM',
        temp: '18°'
    },
    {
        icon: '/images/icon-overcast.webp',
        time: '2 PM',
        temp: '17°'
    },
    {
        icon: '/images/icon-sunny.webp',
        time: '4 PM',
        temp: '17°'
    },
    {
        icon: '/images/icon-overcast.webp',
        time: '6 PM',
        temp: '20°'
    },
    {
        icon: '/images/icon-storm.webp',
        time: '8 PM',
        temp: '19°'
    },
    {
        icon: '/images/icon-overcast.webp',
        time: '10 PM',
        temp: '15°'
    },
    {
        icon: '/images/icon-overcast.webp',
        time: '10 PM',
        temp: '15°'
    }
])
</script>

<style lang="scss" scoped>
</style>