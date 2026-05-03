<template>
    <li class="hero__forecast-hourly-title-week-days-dropdown-item" @click="handleClickDay(dayName)">{{ dayName }}</li>
</template>

<script setup>
import { ref } from 'vue';

    defineProps({
        dayName: String
    })

    const emit = defineEmits(['select-day'])

    const handleClickDay = (dayName) => {
        emit('select-day', dayName)
    }
</script>

<style lang="scss" scoped>

</style>