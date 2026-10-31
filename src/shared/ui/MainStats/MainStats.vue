<template>
    <div class="hero__forecast-main-stats-item">
        <span class="hero__forecast-main-stats-item-name">
            {{ name }}
        </span>

        <span class="hero__forecast-main-stats-item-value-fl">
            {{ value }}
        </span>
    </div>
</template>

<script setup>
    defineProps({
        name: String,
        value: String
    })
</script>

<style lang="scss" scoped>

</style>