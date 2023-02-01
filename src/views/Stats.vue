<template>
    <div class="stats">
        <div class="card-wrapper">
            <v-card class="card" width="800">
                <v-card-item>
                    <v-card-title>Статистика игры</v-card-title>
                </v-card-item>
                <v-card-text style="line-height: normal">
                    <div class="inner-stats">
                        <div class="title">
                            Имя пользователя: {{ currentName }}
                        </div>
                        <div class="block">
                            <div class="left-block">
                                <div class="count"> Количество вопросов: {{ currentTotalAnswers }} </div>
                                <div class="count"> Количество баллов: {{ currentScore }} </div>
                            </div>
                            <div class="right-block">
                                <div class="count"> Количество правильных ответов: {{ currentCountRightAnswers }} </div>
                                <div class="count"> Количество неправильных ответов: {{ currentCountWrongAnswers }}
                                </div>
                            </div>
                        </div>
                    </div>
                </v-card-text>
                <v-card-text style="line-height: normal" v-for="stat in stats">
                    <div class="inner-stats">
                        <div class="title">
                            Имя пользователя: {{ stat.name }}
                        </div>
                        <div class="block">
                            <div class="left-block">
                                <div class="count"> Количество вопросов: {{ stat.total }} </div>
                                <div class="count"> Количество баллов: {{ stat.UserScore }} </div>
                            </div>
                            <div class="right-block">
                                <div class="count"> Количество правильных ответов: {{ stat.right }} </div>
                                <div class="count"> Количество неправильных ответов: {{ stat.wrong }} </div>
                            </div>
                        </div>
                    </div>
                </v-card-text>
            </v-card>
        </div>
    </div>
</template>

<script setup lang="ts">
import { onMounted, Ref, ref } from 'vue';

interface IStats {
    name: string,
    total: number,
    UserScore: number,
    right: number,
    wrong: number
}

interface IAnswers {
    id: number,
    answered: boolean,
    userScore: number,
    color: string
}

const currentName: Ref<string> = ref('')
const currentTotalAnswers: Ref<number> = ref(0)
const currentCountRightAnswers: Ref<number> = ref(0)
const currentCountWrongAnswers: Ref<number> = ref(0)
const currentScore: Ref<number> = ref(0)

let stats: Ref<IStats[]> = ref([]);

const compute = (answers: IAnswers[]) => {
    return {
        total: answers.length,
        right: answers.filter((item: { color: string; }) => item.color === 'green').length,
        wrong: answers.filter((item: { color: string; }) => item.color === 'red').length,
        UserScore: answers.reduce((acc, current) => {
            if (current.color == 'green') {
                return acc + current.userScore;
            } else {
                return acc - current.userScore;
            }
        }, 0)
    }
}


onMounted(() => {
    currentName.value = JSON.parse(`${localStorage.getItem('name')}`);
    const answers = JSON.parse(`${localStorage.getItem('answers')}`)
    const { UserScore, total, right, wrong } = compute(answers)
    currentTotalAnswers.value = total;
    currentCountRightAnswers.value = right;
    currentCountWrongAnswers.value = wrong;
    currentScore.value = UserScore;
    stats.value = JSON.parse(`${localStorage.getItem('statistics')}`)
})


</script>

<style scoped>
.stats {
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 50px 0;
}

.title {
    display: flex;
    justify-content: center;
    font-size: 18px;
}

.block {
    display: flex;
    justify-content: space-between;
    padding-top: 16px;
}

.count {
    padding-top: 10px;
}
</style>
