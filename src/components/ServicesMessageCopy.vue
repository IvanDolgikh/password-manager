<template>
    <div class="message">
        <div class="message__container">
            <p v-if="props.isLogin">Логин скопирован в буфер обмена</p>
            <p v-else>Пароль скопирован в буфер обмена</p>
        </div>
    </div>
</template>

<script setup lang="ts">

const props = defineProps<{
    isLogin: boolean,
}>()

</script>

<style scoped lang="scss">
.message {
    &__container {
        padding: 20px;
        background-color: #dfdfdf;
        position: absolute;
        z-index: 200;
        bottom: 2%;
        left: 2%;
        border-radius: 10px;
        color: #000000;
    }
}
</style>