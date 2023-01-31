<template>
    <div class="navbar">
        <div class="logo">
            <h2>Jeopardy</h2>
        </div>
        <div class="navbar__btns" v-if="route.path !== '/'">
            <router-link activeClass="activeLink" to="/game"> Играть </router-link>
            <router-link activeClass="activeLink" to="/stats"> Статистика </router-link>
            <router-link activeClass="activeLink" to="/" :onClick="logout"> Выйти из игры </router-link>
        </div>
    </div>
</template>

<script lang="ts" setup>
import { useRoute } from 'vue-router';

const compute = (answers: any[]) => {
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


const route = useRoute();
const logout = () => {
    localStorage.setItem('isGameStarted', JSON.stringify(false))
    
    const ArrStats = JSON.parse(`${localStorage.getItem('statistics')}`)
    if (ArrStats == null) {
        localStorage.setItem('statistics', JSON.stringify([]))
    } else {
        const prevAnswers = JSON.parse(`${localStorage.getItem('answers')}`)
        const prevStats = JSON.parse(`${localStorage.getItem('statistics')}`)
        const name = JSON.parse(`${localStorage.getItem('name')}`);
        const { UserScore, total, right, wrong } = compute(prevAnswers);
        localStorage.setItem('statistics', JSON.stringify([...prevStats, { name, total, UserScore, right,  wrong }]))
        localStorage.setItem('answers', JSON.stringify([]))
    }
    localStorage.setItem('name', JSON.stringify(''))
}

</script>

<style scoped>
.activeLink {
    color: red !important;
}

.navbar {
    height: 50px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    background-color: #151b4b;
    color: #fff;
}

.logo {
    margin: 0 30px;
    color: #fff;
}

.navbar__btns {
    margin: 0 30px;
}

.navbar__btns a {
    text-decoration: none;
    color: #fff;
}
</style>