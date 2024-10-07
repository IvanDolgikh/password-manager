<template>
    <div class="info">
        <div class="info__overlay">
            <div class="info__modal">
                <button class="info__close-button" @click="$emit('closeModal', props.isOpenInfoModal)">
                    <span class="pi pi-times"></span>
                </button>
                <p class="info__service-name-title">{{ `Сервис: ${props.data.serviceName}` }}</p>

                <p class="info__service-login-title">Логин</p>
                <div class="info__login-container">
                    <p>{{ props.data.login }}</p>
                    <button @click="copyData(props.data.login)">
                        <span class="pi pi-clone"></span>
                    </button>
                </div>

                <p class="info__service-password-title">Пароль</p>
                <div class="info__password-container">
                    <p>{{ showPasssword }}</p>
                    <button @click="isOpenPassword = !isOpenPassword">
                        <span class="pi pi-eye"></span>
                    </button>
                    <button @click="copyData(props.data.password)">
                        <span class="pi pi-clone"></span>
                    </button>
                </div>

                <button class="info__delete-button" @click="$emit('deleteService', props.data)">Удалить</button>
            </div>
        </div>

        <transition>
            <ServicesMessageCopy v-if="isMessageCopyPopover" :isLogin="isLogin" />
        </transition>
    </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'
import ServicesMessageCopy from './ServicesMessageCopy.vue'

interface IData {
    serviceName: string;
    login: string;
    password: string
}

const props = defineProps<{
    data: IData,
    isOpenInfoModal: boolean
}>()

const isMessageCopyPopover = ref<boolean>(false)
const isLogin = ref<boolean>(false)
const isOpenPassword = ref<boolean>(false)

const showMessage = (): void => {
    isMessageCopyPopover.value = true
    setTimeout(() => {
        isMessageCopyPopover.value = false
    }, 2000)
}

const copyData = (data: string): void => {
    navigator.clipboard.writeText(data)
        .then(() => {
            if (props.data.login === data) {
                isLogin.value = true
            } else {
                isLogin.value = false
            }
        })
        .catch(err => {
            console.error('Ошибка копирования:', err);
        })
        .finally(() => {
            showMessage()
        })
}

const showPasssword = computed(() => {
    if (isOpenPassword.value) {
        return props.data.password
    } else {
        return props.data.password.replace(/./g, '•')
    }
})
</script>

<style scoped lang="scss">
.info {
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
        row-gap: 18px;
        width: 500px;
        height: auto;
        background-color: #262626;
        border-radius: 14px;
        padding: 10px 20px 20px 20px;
    }

    &__close-button {
        display: block;
        background-color: transparent;
        border: none;
        padding: 8px;
        color: #ffffff;
        font-size: 18px;
        margin: 0 0 20px auto;
        cursor: pointer;
    }

    &__service-name-title {
        font-size: 20px;
        font-weight: 300;
        margin-bottom: 20px;
    }

    &__service-login-title,
    &__service-password-title {
        font-weight: 300;
        font-size: 16px;
        margin-bottom: 12px;
    }

    &__login-container,
    &__password-container {
        display: grid;
        grid-template-columns: 1fr min-content min-content;
        padding: 6px 12px;
        margin-bottom: 20px;
        background-color: #82828229;
        width: 60%;

        p {
            font-size: 13px;
            margin-right: 20px;
            color: #c1c1c1;
        }

        button {
            display: flex;
            justify-content: center;
            align-items: center;
            background-color: transparent;
            border: none;
            color: #ffffff;
            font-size: 14px;
            cursor: pointer;
            margin-left: auto;
        }
    }

    &__password-container {
        margin-bottom: 40px;

        button {
            margin-left: auto;
        }
    }

    &__delete-button {
        display: block;
        background-color: #00484e;
        border: none;
        padding: 6px 14px;
        width: 26%;
        color: #ffffff;
        margin: auto auto 0 auto;
        cursor: pointer;
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