<script setup>
import { iconRegistry } from './registry';
import { computed } from 'vue';

const props = defineProps({
    name: {
        type: String,
    }
})

const src = computed(() => iconRegistry[props.name])
</script>

<template>
    <img :src="src"/>
</template>