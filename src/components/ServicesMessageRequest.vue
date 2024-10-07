<template>
    <div class="message">
        <div class="message__container">
            <p class="message__success" v-if="props.isSuccessRequest">Данные успешно отправлены!</p>
            <p class="message__error" v-else="!props.isSuccessRequest">Произошла ошибка, попробуйте еще раз!</p>

        </div>
    </div>
</template>

<script setup lang="ts">

const props = defineProps<{
    isSuccessRequest: boolean;
}>()

</script>

<style scoped lang="scss">
.message {
    &__container {
        padding: 20px;
        background-color: #e9e9e9;
        z-index: 200;
    }

    &__success {
        color: green;
    }

    &__error {
        color: red;
    }
}
</style>