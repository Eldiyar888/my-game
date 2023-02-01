<template>
        <div class="entry">
            <v-form>
                <v-card width="500px">
                    <v-card-text>
                        <v-text-field v-model="name" clearable hide-details="auto" label="Введите имя"></v-text-field>
                    </v-card-text>
                    <v-card-text>
                        <v-btn height="50px" @click="handleClick" block color="success">
                            Войти
                        </v-btn>
                    </v-card-text>
                </v-card>
            </v-form>
        </div>
</template>

<script setup lang="ts">
import router from '@/router';
import { onMounted, Ref, ref } from 'vue';

const name: Ref<string> = ref('');

const handleClick = () => {
        let regexp = /^[а-яА-ЯёЁa-zA-Z0-9-_]+$/
        if (name.value && regexp.test(name.value) && name.value.length > 1) {
            router.push({path: '/game'})
            localStorage.setItem('name', JSON.stringify(name.value));
            localStorage.setItem('isGameStarted', JSON.stringify(true));
        }
        else {
            alert('От двух символов, разрешены английские и русские буквы, числа и знак подчеркивания')
        }
    }

    onMounted(() => {
       const isStarted = localStorage.getItem('isGameStarted')
    })
</script>

<style scoped>
.entry {
    height: calc(100vh - 100px);
    display: flex;
    align-items: center;
    justify-content: center;
}
</style>
