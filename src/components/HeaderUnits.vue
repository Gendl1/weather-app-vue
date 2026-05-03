<template>
    <div class="header__units" @click="toggleOpen" ref="dropdownRef">
        <div class="header__units-img">
            <img src="/images/units-img.svg">
        </div>

        <div class="header__units-text">
            <span>Units</span>
        </div>

        <div class="header__units-arrow">
            <img src="/images/units-arrow.svg">
        </div>

        <div class="header__units-dropdown" :class="{ open: isOpen }">
            <button class="header__units-dropdown-btn">
                Switch to Imperial
            </button>

            <div class="header__units-dropdown-params">
                <UnitsDropdownItem
                v-for="item in headerUnitsValues"
                :itemName="item.itemName"
                :itemValueMetric="item.itemValueMetric"
                :itemValueImper="item.itemValueImper"
                :itemIdMetric="item.itemIdMetric"
                :itemIdImper="item.itemIdImper"
                />
            </div>
        </div>
    </div>
</template>

<script setup>
import { computed, ref } from 'vue';
import UnitsDropdownItem from './UnitsDropdownItem.vue';
import { onClickOutside } from '@vueuse/core';

const isOpen = ref(false)
const dropdownRef = ref(null)

const toggleOpen = () => {
    isOpen.value = !isOpen.value
}

onClickOutside(dropdownRef, () => {
    isOpen.value = false
})

const headerUnitsValues = computed(() => [
    {
        itemName: 'Temperature',
        itemValueMetric: 'Celsius (°C)',
        itemValueImper: 'Fahrenheit (°F)',
        itemIdMetric: 'unit-cels',
        itemIdImper: 'unit-fahr'
    },
    {
        itemName: 'Wind Speed',
        itemValueMetric: 'km/h',
        itemValueImper: 'mph',
        itemIdMetric: 'unit-kmh',
        itemIdImper: 'unit-mph'
    },
    {
        itemName: 'Precipitation',
        itemValueMetric: 'Millimeters (mm)',
        itemValueImper: 'Inches (in)',
        itemIdMetric: 'unit-mm',
        itemIdImper: 'unit-in'
    }
])
</script>

<style lang="scss" scoped>

</style>