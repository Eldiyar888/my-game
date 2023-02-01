<template>
  <div class="wrapper-game">
    <div class="game">
      <div>
        <v-card style="color: gold;" color="#151b4b">
          <v-card-title>
            <h3>Заработанные баллы: {{ score }}</h3>
          </v-card-title>
        </v-card>
      </div>
      <div>
        <game-item v-for="post in posts" :post="post" :totalValue="totalValue" />
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, onMounted, Ref } from 'vue';
import GameItem from '../components/TitleItem.vue';
import Navbar from '@/components/Navbar.vue';
import axios from 'axios';

export interface IClues {
  id: number,
  answer: string,
  question: string,
  value: number
}

export interface IPosts {
  title: string,
  clues: IClues[]
}

export default defineComponent({
  components: {
    GameItem,
    Navbar
  },
  setup() {

    const posts: Ref<IPosts[]> = ref([]);
    const score = ref(0)

    const totalScore = () => {
      const ArrAnswers = JSON.parse(`${localStorage.getItem('answers')}`)
      ArrAnswers.forEach((Answer: { color: string; userScore: number; }) => {
        if (Answer.color == 'green') {
          score.value += Answer.userScore;
        } else if (Answer.color == 'red') {
          score.value -= Answer.userScore;
        }
      });
    }

    const totalValue = (value: number, bool: boolean) => {
      bool ? score.value += value : score.value -= value
    }
    const fetchPosts = async () => {
      const response = await axios.get('http://jservice.io/api/clues?min_date=1985-02-20');
      const dataObj: any = {};
      const answers = response.data.filter((answer: { value: null; }) => answer.value !== null)
      answers.forEach((item: { category: { title: string | number; }; }) => {
        dataObj[item.category.title] = []
      })

      Object.keys(dataObj).forEach((obj) => {
        answers.forEach((item: { category: { title: string; }; }) => {
          if (item.category.title === obj) {
            dataObj[obj].push(item)
          }
          dataObj[obj].sort(
            (a: { value: number }, b: { value: number }) => a.value - b.value
          )
        })
        if (dataObj[obj].length != 5) {
          delete dataObj[obj]
        }
      })

      const data = [];

      for (const key in dataObj) {
        const obj = {
          title: key,
          clues: dataObj[key]
        }
        data.push(obj);
      }
      posts.value = data;
    }

    onMounted(fetchPosts);
    onMounted(() => {
      totalScore();
    })
    return {
      posts, score, totalValue
    }
  }
})
</script>

<style scoped>
.wrapper-game {
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: space-evenly;
}

.game {
  min-height: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-evenly;
}
</style>