<template>
    <div class="hero__search">
        <div class="hero__search-input">
            <div class="hero__search-input-img">
                <UiIcon name="search"/>
            </div>

            <input v-model="cityInput" 
            class="hero__search-input-field" 
            placeholder="Search for a place..." 
            @keyup.enter="handleSearch">
        </div>

        <button 
        class="hero__search-btn" 
        @click="handleSearch">
            Search
        </button>
    </div>
</template>

<script setup>
import { ref } from 'vue';
import UiIcon from '@/shared/ui/Icon/ui-icon.vue';

const cityInput = ref('')

const handleSearch = () => {
    const cityName = cityInput.value.trim()

    if (cityName === '') return

    emit('search', cityName)
}

const emit = defineEmits(['search'])


</script>

<style lang="scss" scoped>

</style>