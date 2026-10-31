<template>
    <aside class="hero__forecast-hourly">
        <div class="hero__forecast-hourly-title">
            <div class="hero__forecast-hourly-title-name">
            Hourly forecast
            </div>

            <HourlyTitleWeek
            @select-day="onSelectDay"
            />
            
        </div>

        <div class="hero__forecast-hourly-grid">
            <HourlyGrid
            v-for="(card, index) in hourly"
            :key="index"
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
import HourlyTitleWeek from './HourlyTitleWeek.vue'

const props = defineProps({
    hourly: {
        type: Array,
        required: true
    }
})

const emit = defineEmits(['select-day'])

function onSelectDay(dayName) {
    emit('select-day', dayName)
}

</script>

<style lang="scss" scoped>
</style>