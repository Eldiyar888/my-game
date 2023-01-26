<template>
    <div>
        <v-row justify="space-around">
            <v-col cols="auto">
                <v-dialog v-model="dialog" fullscreen transition="dialog-bottom-transition">
                    <template v-slot:activator="{ props }">
                        <v-btn :disabled="answer.length > 0 || msg == 'Вы не ответили на вопрос'" class="value-inner"
                            :class="{ rightBg: idValue == post.id && msg == 'Вы ответили правильно', wrongBg: idValue == post.id && msg == 'Вы ответили не правильно' || msg == 'Вы не ответили на вопрос' }"
                            v-bind="props" @click="startTimer(post.id)"> {{
                                post.value == null ? '500' : post.value
                            }}
                        </v-btn>
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
                                    <v-card v-if="isCheckAnswer">
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
                                    <v-card v-else>
                                        <v-alert style="font-size: 20px;" border="top" border-color="#151b4b"
                                            elevation="2">
                                            <p class="post"> {{ post.question }} </p>
                                            <p class="post"
                                                :class="{ right: msg == 'Вы ответили правильно', wrong: msg == 'Вы ответили не правильно' }">
                                                {{ msg }}</p>
                                            <p class="post right">Правильный ответ: {{ post.answer }}</p>
                                        </v-alert>
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
import { defineProps, ref } from 'vue';
const emit = defineEmits(['customChange'])
interface Post {
    id: number
    value: number
    question: string
    answer: string
}
interface ItemProps {
    post: Post
}
const { post } = defineProps<ItemProps>()

let timer = ref(5);
let intervalId: any = null
const dialog = ref(false)
const idValue = ref(null)
const answer: any = ref('');
const msg: any = ref('');
const isCheckAnswer: any = ref(true);
const score: any = ref(0);

const close = () => {
    dialog.value = false;
    clearInterval(intervalId);
}

const setAnswersToLocalStorage = (idValue: any, score: any, color: any) => {
    const ArrAnswers = JSON.parse(`${localStorage.getItem('answers')}`)
    if (ArrAnswers == null) {
        localStorage.setItem('answers', JSON.stringify([]))
    } else {
        const prevAnswers = JSON.parse(`${localStorage.getItem('answers')}`)
        localStorage.setItem('answers', JSON.stringify([...prevAnswers, { idValue, score, color }]))
    }
}

const startTimer = (id: any) => {

    idValue.value = id;

    intervalId = setInterval(() => {
        if (timer.value > 0) {
            timer.value--
            if (isCheckAnswer.value == false && timer.value > 0) {
                clearInterval(intervalId);
                // close();
            }
        } else {
            clearInterval(intervalId);
            isCheckAnswer.value = false;
            msg.value = 'Вы не ответили на вопрос';
            score.value -= post.value;
            emit('customChange', score)
            setAnswersToLocalStorage(idValue, score, 'red');
            // close();
        }
    }, 1000)
}

const checkAnswer = () => {
    if (answer.value) {
        isCheckAnswer.value = false;
        if (answer.value == post.answer) {
            msg.value = 'Вы ответили правильно';
            score.value += post.value;
            emit('customChange', score)
            setAnswersToLocalStorage(idValue, score, 'green');
        }
        else {
            msg.value = 'Вы ответили не правильно';
            score.value -= post.value;
            emit('customChange', score)
            setAnswersToLocalStorage(idValue, score, 'red');
        }
    }
}

</script>


<style scoped>
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

.rightBg {
    background-color: green;
}

.wrong {
    color: red;
}

.wrongBg {
    background-color: red;
}
</style>