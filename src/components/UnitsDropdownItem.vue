<template>
    <div class="header__units-dropdown-params-item">
        <div class="header__units-dropdown-params-item-name">
            {{ itemName }}
        </div>
        <div class="header__units-dropdown-params-item-value-metric" :id="itemIdMetric" :class="{active: isMetric}">
            {{ itemValueMetric }}
        </div>
        <div class="header__units-dropdown-params-item-value-imper" :id="itemIdImper" :class="{active: !isMetric}">
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
    itemIdImper: String,
    isMetric: Boolean
})

</script>

<style lang="scss" scoped>
</style>