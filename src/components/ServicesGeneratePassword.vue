<template>
    <div class="generate-password">
        <div class="generate-password__container">

            <label>
                <input type="number" v-model="passwordLength" min="1" placeholder="Длинна пароля">
            </label>

            <label>
                <input type="checkbox" v-model="useLetters">
                <span>Буквы</span>
            </label>

            <label>
                <input type="checkbox" v-model="useNumbers">
                <span>Цифры</span>
            </label>

            <label>
                <input type="checkbox" v-model="useSymbols">
                <span>Спецсимволы</span>
            </label>

            <label>
                <select class="generate-password__select" v-model="caseType">
                    <option value="lowercase">Нижний регистр</option>
                    <option value="uppercase">Верхний регистр</option>
                    <option value="random">Случайный</option>
                </select>
            </label>

            <button @click="generatePassword">Сгенерировать пароль</button>
        </div>

    </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'

const passwordLength = ref<number>(12);
const useLetters = ref<boolean>(true);
const useNumbers = ref<boolean>(true);
const useSymbols = ref<boolean>(true);
const caseType = ref('random');

const generatedPassword = ref<string>('');

const emit = defineEmits(['getPassword'])

const generatePassword = () => {
    generatedPassword.value = generatePasswordFromSettings();
    emit('getPassword', generatedPassword.value)
};

const generatePasswordFromSettings = (): string => {
    const characters: string[] = [];
    let password: string = '';

    if (useLetters.value) {
        characters.push(caseType.value === 'lowercase' ? 'abcdefghijklmnopqrstuvwxyz' : caseType.value === 'uppercase' ? 'ABCDEFGHIJKLMNOPQRSTUVWXYZ' : 'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ');
    }
    if (useNumbers.value) {
        characters.push('0123456789');
    }
    if (useSymbols.value) {
        characters.push('!@#$%^&*()_-+=`~|}{[]\:;?><,./');
    }
    for (let i = 0; i < passwordLength.value; i++) {
        const randomCharacterSetIndex = Math.floor(Math.random() * characters.length);
        const randomCharacterIndex = Math.floor(Math.random() * characters[randomCharacterSetIndex].length);
        password += characters[randomCharacterSetIndex][randomCharacterIndex];
    }
    return password;
};

</script>

<style scoped lang="scss">
.generate-password {

    &__container {
        display: flex;
        align-items: center;
        flex-wrap: wrap;
        gap: 22px 20px;
    }

    label {
        display: block;

        span {
            margin-left: 10px;
            font-size: 14px;
        }

        input[type="number"] {
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

    button {
        width: 30%;
        border: none;
        padding: 6px 10px;
        background-color: #00484e;
        color: #ffffff;
        cursor: pointer;
    }

    &__select {
        background-color: #505050;
        color: azure;
        border: none;
        padding: 5px;
    }
}
</style>
