<template>
    <div class="services-item" @click="isOpenInfoModal = true">
        <div class="services-item__container">
            <p>{{ props.data.serviceName }}</p>
            <button>
                <span class="pi pi-angle-right"></span>
            </button>
        </div>

        <Teleport to="body">
            <transition>
                <ServicesInfo v-if="isOpenInfoModal" :data="props.data" :isOpenInfoModal="isOpenInfoModal"
                    @close-modal="isOpenInfoModal = false" @delete-service="deleteService" />
            </transition>
        </Teleport>


        <transition>
            <ServicesMessageRequest class="services-item__message" v-if="message"
                :isSuccessRequest="isSuccessRequest" />
        </transition>
    </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'

import ServicesInfo from './ServicesInfo.vue'
import ServicesMessageRequest from './ServicesMessageRequest.vue'
import { useStorage } from '@vueuse/core'

interface IData {
    serviceName: string;
    login: string;
    password: string
}

const isOpenInfoModal = ref<boolean>(false)
const isSuccessRequest = ref<boolean>(false)
const message = ref<boolean>(false)

const props = defineProps<{
    data: IData
}>()

const showMessage = (): void => {
    message.value = true
    setTimeout(() => {
        message.value = false
    }, 2000)
}

const arrData = useStorage<IData[]>('services', () => []);
const deleteService = (service: IData): void => {
    setTimeout(() => {
        showMessage()
        const randomValue = Math.random();
        if (randomValue > 0.5) {
            isSuccessRequest.value = true
            const index = arrData.value.findIndex(item => item.serviceName === service.serviceName);
            if (index !== -1) {
                arrData.value.splice(index, 1);

                setTimeout(() => {
                    isOpenInfoModal.value = false
                }, 2000)
            }
        } else {
            isSuccessRequest.value = false
        }
    }, 1000);
};

</script>

<style scoped lang="scss">
.services-item {
    cursor: pointer;

    &__container {
        display: flex;
        justify-content: space-between;
        column-gap: 30px;
        padding: 0 0 10px 0;
        margin-bottom: 20px;
        border-bottom: 1px solid #8c8c8c;
    }

    p {
        color: #ffffff;
    }

    button {
        background-color: transparent;
        color: #ffffff;
        border: none;
        padding: 0;
        font-size: 16px;
        cursor: pointer;
    }

    &__message {
        position: absolute;
        top: 0;
        left: 0;
        right: 0;
        z-index: 200;
    }
}

.v-enter-active,
.v-leave-active {
    transition: opacity 0.2s ease;
}

.v-enter-from,
.v-leave-to {
    opacity: 0;
}

.v-enter-to,
.v-leave-from {
    opacity: 1;
}
</style>