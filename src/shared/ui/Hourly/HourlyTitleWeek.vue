<template>
    <div class="hero__forecast-hourly-title-week">   
        <div class="hero__forecast-hourly-title-week-day" @click="toggleOpen" ref="dropdownRef">
            <span class="hero__forecast-hourly-title-week-day-name">
                {{ selectedDay }}
            </span>

            <div class="hero__forecast-hourly-title-week-day-img">
                <UiIcon name="dropdown"/>
            </div>
        </div>

        <ul class="hero__forecast-hourly-title-week-days-dropdown" :class="{ open: isOpen }">
            <HourlyTitleWeekDropdown
            v-for="day in weekDays"
            :dayName="day.dayName"
            @select-day="selectNewDay"
            />
        </ul>
    </div>
</template>

<script setup>
import { computed, ref } from 'vue'
import HourlyTitleWeekDropdown from './HourlyTitleWeekDropdown.vue';
import UiIcon from '@/shared/ui/Icon/ui-icon.vue';
import { onClickOutside } from '@vueuse/core';

const isOpen = ref(false)
const dropdownRef = ref(null)
const selectedDay = ref('Tuesday')
const emit = defineEmits(['select-day'])

const selectNewDay = (newDay) => {
    selectedDay.value = newDay
    isOpen.value = false
    emit('select-day', newDay)  
}

const toggleOpen = () => {
    isOpen.value = !isOpen.value
}

onClickOutside(dropdownRef, () => {
    isOpen.value = false
})

const weekDays = computed(() => [
    {
        dayName: 'Monday'
    },
    {
        dayName: 'Tuesday'
    },
    {
        dayName: 'Wednesday'
    },
    {
        dayName: 'Thursday'
    },
    {
        dayName: 'Friday'
    },
    {
        dayName: 'Saturday'
    },
    {
        dayName: 'Sunday'
    }
])

</script>

<style lang="scss" scoped>

</style>