<template>
    <div class="hero__forecast-hourly-grid-item">
        <div class="hero__forecast-hourly-grid-item-info">
            <div class="hero__forecast-hourly-grid-item-info-img-1">
                <UiIcon :name="icon"/>
            </div>

            <span class="hero__forecast-hourly-grid-item-info-time">
                {{ time }}
            </span>
        </div>
        <span class="hero__forecast-hourly-grid-item-temp-1">
            {{ temp }}
        </span>
    </div>
</template>

<script setup>
import UiIcon from '@/shared/ui/Icon/ui-icon.vue';

defineProps({
    icon: String,
    time: String,
    temp: String
})

</script>

<style lang="scss" scoped>

</style>