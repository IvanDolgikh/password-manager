<template>
    <div class="main">
        <div class="main__container">
            <Services />
        </div>

    </div>
</template>

<script setup lang="ts">
import Services from './views/Services.vue'

</script>

<style scoped lang="scss">
.main {
    &__container {
        height: 100vh;
        display: flex;
        justify-content: center;
        align-items: center;
    }
}
</style>