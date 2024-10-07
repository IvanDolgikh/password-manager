<template>
    <div class="services-search">
        <div class="services-search__container">
            <input v-model="text" @input="search" type="text" placeholder="Type service...">
        </div>

    </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'

const text = ref<string>('')

const emit = defineEmits(['search']);

const search = () => {
    emit('search', text.value);
};
</script>

<style scoped lang="scss">
.services-search {
    input {
        background-color: transparent;
        border: 1px solid #6a6a6a;
        border-radius: 6px;
        padding: 8px;
        color: #ffffff;
        font-family: 'Montserrat', sans-serif;
        font-weight: 400;
    }
}
</style>