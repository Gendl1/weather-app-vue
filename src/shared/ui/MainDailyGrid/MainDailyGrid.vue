<template>
    <div class="hero__forecast-main-daily-grid-item">
        <span class="hero__forecast-main-daily-grid-item-day-1">
            {{ dayName }}
        </span>

        <div class="hero__forecast-main-daily-grid-item-img-1">
            <UiIcon  :name="icon"/>
        </div>

        <div class="hero__forecast-main-daily-grid-item-temp-1">
            <span id="daily-temp-1-max">{{ dayTempMax }}</span>
            <span id="daily-temp-1-min">{{ dayTempMin }}</span>
        </div>
    </div>
</template>

<script setup>
import UiIcon from '@/shared/ui/icon/ui-icon.vue';

defineProps({
    dayName: String,
    icon: String,
    dayTempMax: String,
    dayTempMin: String
})
</script>

<style lang="scss" scoped>

</style>