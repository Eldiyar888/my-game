<template>
    <div class="value-item">
        <v-row justify="space-around">
            <v-col cols="auto">
                <v-dialog v-model="dialog" fullscreen transition="dialog-bottom-transition">
                    <template v-slot:activator="{ props }">
                        <v-btn :disabled="isAnswered || msg == 'Вы не ответили на вопрос'" class="value-inner"
                            :class="color?.color" v-bind="props" @click="startTimer()"> {{
                                post.value
                            }}
                        </v-btn>
                        <v-snackbar color="#151b4b" vertical :timeout="3000" v-model="snackbar" multi-line>
                            <div :class="{ right: msg == 'Вы ответили правильно', wrong: msg == 'Вы ответили не правильно' }"> {{ msg }}</div>
                            <p> Вопрос: {{ post.question }}</p>
                            <p> Правильный ответ: {{ post.answer }}</p>
                            <p> {{ result }} </p>
                            <template v-slot:actions>
                                <v-btn color="red" variant="text" @click="snackbar = false">
                                    Close
                                </v-btn>
                            </template>
                        </v-snackbar>
                    </template>
                    <template v-slot:default="{ isActive }">
                        <v-card>
                            <v-toolbar color="#151b4b" style="color: #fff">
                                <v-toolbar-title>Если вы не ответите в течении заданного времени ваш ответ будет
                                    недействительным {{
                                        timer
                                    }}</v-toolbar-title>
                            </v-toolbar>
                            <v-card-text class="card-text">
                                <div class="text-h2 pa-12">
                                    <v-card>
                                        <v-card-text>
                                            <h1>{{ post.question }} </h1>
                                        </v-card-text>
                                        <v-card-text>
                                            <v-text-field v-model="answer" clearable hide-details="auto"
                                                label="Введите ответ"></v-text-field>
                                        </v-card-text>
                                        <v-card-text>
                                            <v-btn height="50px" block @click="checkAnswer" color="success">
                                                Подтвердить
                                            </v-btn>
                                        </v-card-text>
                                    </v-card>
                                </div>
                            </v-card-text>
                            <v-card-actions class="justify-end">
                                <v-btn variant="text" @click="close">Close</v-btn>
                            </v-card-actions>
                        </v-card>
                    </template>
                </v-dialog>
            </v-col>
        </v-row>
    </div>
</template>

<script lang="ts" setup>
import { computed, defineProps, onMounted, onUpdated, Ref, ref, watch } from 'vue';

export interface IColor {
    id: number,
    answered: boolean,
    userScore: number,
    color: string
}

interface Post {
    id: number
    value: number
    question: string
    answer: string
}

interface ItemProps {
    post: Post
    totalValue: (value: number, bool: boolean) => void
}

const { post, totalValue } = defineProps<ItemProps>()

const seconds: number = 5;
let timer: Ref<number> = ref(seconds);
let intervalId: number | undefined = 0
const dialog: Ref<boolean> = ref(false)
let answer: string = '';
const msg: Ref<string> = ref('');
const isCheckAnswer: Ref<boolean> = ref(true);
let color: IColor = {
    id: 0,
    answered: false,
    userScore: 0,
    color: ''
}
const isAnswered: Ref<boolean> = ref(false);

let snackbar: any = ref(false);

const result = computed(() => msg.value === 'Вы ответили правильно' ? `Вы заработали ${post.value} очков` : `Вы потеряли ${post.value} очков`);

const close = () => {
    dialog.value = false;
    clearInterval(intervalId);
    timer.value = seconds;
}

const setAnswersToLocalStorage = (id: number, answered: boolean, userScore: number, color: string) => {
    const ArrAnswers = JSON.parse(`${localStorage.getItem('answers')}`)
    if (ArrAnswers == null) {
        localStorage.setItem('answers', JSON.stringify([]))
    } else {
        const prevAnswers = JSON.parse(`${localStorage.getItem('answers')}`)
        localStorage.setItem('answers', JSON.stringify([...prevAnswers, { id, answered, userScore, color }]))
    }
}

const addDataAnswers = (id: number) => {
    const ArrAnswers = JSON.parse(`${localStorage.getItem('answers')}`)
    const itemId = ArrAnswers.find((answerItem: { id: number; answered: boolean, userScore: number, color: string }) => answerItem.id == id);
    isAnswered.value = itemId?.answered;
    color = itemId;
}

const startTimer = () => {
    intervalId = setInterval(() => {
        if (timer.value > 0) {
            timer.value--
            if (isCheckAnswer.value == false && timer.value > 0) {
                clearInterval(intervalId);
            }
        } else {
            clearInterval(intervalId);
            isCheckAnswer.value = false;
            msg.value = 'Вы не ответили на вопрос';
            setAnswersToLocalStorage(post.id, true, post.value, 'red');
            addDataAnswers(post.id);
            totalValue(post.value, false)
            close();
            snackbar.value = true;
        }
    }, 1000)
}

const checkAnswer = () => {
    if (answer) {
        isCheckAnswer.value = false;
        if (answer == post.answer) {
            msg.value = 'Вы ответили правильно';
            setAnswersToLocalStorage(post.id, true, post.value, 'green');
            addDataAnswers(post.id);
            totalValue(post.value, true)
        }
        else {
            msg.value = 'Вы ответили не правильно';
            setAnswersToLocalStorage(post.id, true, post.value, 'red');
            addDataAnswers(post.id);
            totalValue(post.value, false)
        }
    }
    close();
    snackbar.value = true;
}

addDataAnswers(post.id);

</script>

<style>
.value-item {
    height: 100%;
}

.main-block {
    width: 100%;
    height: 100%;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
}

.v-dialog .v-overlay__content>.v-card>.v-card-text {
    display: flex;
    align-items: center;
    justify-content: center;
}

.v-card .v-card-text {
    line-height: 2.5rem;
}

.post {
    line-height: 2.0;
}

.red {
    background-color: red !important;
    color: #fff !important;
}

.green {
    background-color: green !important;
    color: #fff !important;
}

.value-inner {
    display: flex;
    justify-content: center;
    padding: 30px;
    width: 180px;
    height: 80px;
    margin: 3px;
    color: #fff;
    font-size: 20px;
    cursor: pointer;
    background-color: #151b4b;
}

.pa-12 {
    width: 800px;
}

.block-question {
    font-size: 30px;
}

.right {
    color: green;
}

.wrong {
    color: red;
}
</style>
