<template>
    <div class="hero__forecast-hourly-grid-item">
        <div class="hero__forecast-hourly-grid-item-info">
            <div class="hero__forecast-hourly-grid-item-info-img-1">
                <img :src="icon">
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
defineProps({
    icon: String,
    time: String,
    temp: String
})

</script>

<style lang="scss" scoped>

</style>