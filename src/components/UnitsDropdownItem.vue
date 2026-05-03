<template>
    <div class="header__units-dropdown-params-item">
        <div class="header__units-dropdown-params-item-name">
            {{ itemName }}
        </div>
        <div class="header__units-dropdown-params-item-value-metric active" :id="itemIdMetric">
            {{ itemValueMetric }}
        </div>
        <div class="header__units-dropdown-params-item-value-imper" :id="itemIdImper">
            {{ itemValueImper }}
        </div>
    </div>
</template>

<script setup>
defineProps({
    itemName: String,
    itemValueMetric: String,
    itemValueImper: String,
    itemIdMetric: String,
    itemIdImper: String
})

</script>

<style lang="scss" scoped>
</style>