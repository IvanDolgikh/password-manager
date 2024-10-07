<template>
    <div class="services">
        <div class="services__conatiner">
            <div class="services__controls">
                <ServicesButton @click="isOpenModal = true" />
                <ServicesSearch @search="handleSearch" />
            </div>

            <ServicesList :arrData="arrData" />

            <Teleport to="body">
                <transition name="modal-fade">
                    <ServicesModal v-if="isOpenModal" :isOpenModal="isOpenModal" @close-modal="isOpenModal = false" />
                </transition>
            </Teleport>
        </div>

    </div>
</template>

<script setup lang="ts">
import { ref, watch, reactive } from 'vue'
import ServicesList from '../components/ServicesList.vue'
import ServicesButton from '../components/ServicesButton.vue';
import ServicesSearch from '../components/ServicesSearch.vue'
import ServicesModal from '../components/ServicesModal.vue'
import { useStorage } from '@vueuse/core'

const isOpenModal = ref<boolean>(false)

interface IData {
    serviceName: string;
    login: string;
    password: string
}

const arrData = useStorage<IData[]>('services', () => []);

let copyArrData = ref<IData[]>(arrData.value);

const handleSearch = (text: string): void => {
    if (text) {
        arrData.value = arrData.value.filter(item => {
            return item.serviceName.toLowerCase().includes(text.toLowerCase());
        });
    } else {
        arrData.value = copyArrData.value;
    }
};
</script>

<style scoped lang="scss">
.services {
    &__conatiner {
        padding: 20px;
        background-color: #262626;
        box-shadow: 0px 0px 12px 1px rgba(0, 0, 0, 0.2);
        border-radius: 12px;
        width: 700px;
    }

    &__controls {
        display: flex;
        justify-content: space-between;
        align-items: center;
        margin-bottom: 40px;
    }
}

.modal-fade-enter-active,
.modal-fade-leave-active {
    transition: opacity 0.2s ease;
}

.modal-fade-enter-from,
.modal-fade-leave-to {
    opacity: 0;
}

.modal-fade-enter-to,
.modal-fade-leave-from {
    opacity: 1;
}
</style>