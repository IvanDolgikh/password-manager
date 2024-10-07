<template>
    <div class="services">
        <div class="services__container">
            <ul class="services__list">
                <li v-for="(item, index ) in props.arrData" :key="index">
                    <ServicesItem :data="item" />
                </li>
            </ul>
        </div>
    </div>
</template>

<script setup lang="ts">
import { toRefs } from 'vue'
import ServicesItem from './ServicesItem.vue'

interface IData {
    serviceName: string;
    login: string;
    password: string
}

const props = defineProps<{
    arrData: IData[]
}>()

</script>

<style></style>