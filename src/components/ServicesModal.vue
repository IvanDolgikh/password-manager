<template>
    <div class="services-modal">
        <div class="services-modal__overlay">
            <div class="services-modal__modal">
                <button class="services-modal__close-button" @click="sendEmit">
                    <span class="pi pi-times"></span>
                </button>

                <ServicesGeneratePassword class="services-modal__generate-password" v-if="!isCustomChars"
                    @get-password="getGeneratedPassword" />

                <div class="services-modal__info">
                    <label class="services-modal__label">
                        <span class="pi pi-link"></span>
                        <input type="text" v-model="data.serviceName" placeholder="Название сервиса">
                    </label>

                    <label class="services-modal__label">
                        <span class="pi pi-sign-in"></span>
                        <input type="text" v-model="data.login" placeholder="Логин">
                    </label>

                    <label class="services-modal__label">
                        <span class="pi pi-key"></span>
                        <input type="password" v-model="data.password" placeholder="Пароль">
                    </label>

                    <label class="services-modal__checkbox-label">
                        <input type="checkbox" v-model="isCustomChars">
                        <span>Свой пароль</span>
                    </label>
                </div>

                <button class="services-modal__send-button" @click="sendData">Сохранить</button>
            </div>

            <transition>
                <ServicesMessageRequest class="services-modal__message" v-if="message"
                    :isSuccessRequest="isSuccessRequest" />
            </transition>
        </div>
    </div>
</template>

<script setup lang="ts">
import { reactive, ref } from 'vue'

import ServicesMessageRequest from './ServicesMessageRequest.vue'
import ServicesGeneratePassword from './ServicesGeneratePassword.vue'
import { useStorage } from '@vueuse/core'

interface IData {
    serviceName: string;
    login: string;
    password: string
}

const props = defineProps<{
    isOpenModal: boolean
}>()

const emit = defineEmits(['closeModal']);

const sendEmit = () => emit('closeModal', props.isOpenModal)

const isSuccessRequest = ref<boolean>(false)
const message = ref<boolean>(false)
const isCustomChars = ref<boolean>(true);

const data: IData = reactive({
    serviceName: '',
    login: '',
    password: ''
})

const getGeneratedPassword = (password: string) => {
    data.password = password
}

const showMessage = (): void => {
    message.value = true
    setTimeout(() => {
        message.value = false
    }, 2000)
}
const { value: services } = useStorage<IData[]>('services', () => []);

const sendData = (): void => {
    setTimeout(() => {
        showMessage()
        const randomValue = Math.random();
        if (randomValue > 0.5) {
            isSuccessRequest.value = true
            services.push(data)
            setTimeout(() => {
                sendEmit()
            }, 2000)
        } else {
            isSuccessRequest.value = false
        }
    }, 1000);
};

</script>

<style scoped lang="scss">
.services-modal {
    &__overlay {
        position: fixed;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        background-color: rgba(0, 0, 0, 0.4);
        z-index: 100;
    }

    &__modal {
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
        z-index: 101;
        display: flex;
        flex-direction: column;
        min-width: 50%;
        padding: 0 20px 20px 20px;
        background-color: #262626;
        border-radius: 14px;
    }

    &__generate-password {
        margin-bottom: 40px;
    }

    &__info {
        display: flex;
        flex-direction: column;
        align-items: center;
        row-gap: 20px;
    }


    &__close-button {
        display: block;
        background-color: transparent;
        border: none;
        padding: 8px;
        color: #ffffff;
        font-size: 18px;
        margin: 8px 0 20px auto;
        cursor: pointer;
        grid-column: 2;
        grid-row: 1;
    }

    &__send-button {
        margin: 0 auto;
        width: 30%;
        border: none;
        padding: 6px 10px;
        background-color: #00484e;
        color: #ffffff;
        cursor: pointer;
    }

    &__label {
        display: flex;
        justify-content: center;
        width: 60%;

        span {
            background-color: #00484e;
            padding: 8px;
            color: #ffffff;
        }

        input {
            background-color: #82828229;
            border: none;
            color: #ffffff;
            padding: 4px 4px 4px 10px;
            width: 100%;
            font-family: 'Montserrat', sans-serif;
            font-weight: 300;

            &:focus-visible {
                outline: none;
            }
        }
    }

    &__checkbox-label {
        display: block;
        margin-bottom: 30px;

        span {
            font-size: 13px;
        }

        input {
            margin-right: 10px;
        }
    }

    &__message {
        position: absolute;
        top: 0;
        left: 0;
        right: 0;
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
</style>.