<template>
  <div class="game">
    <div>
      <v-card style="color: gold" color="#151b4b">
        <v-card-title>
          <h3>Заработанные баллы: {{ score }}</h3>
        </v-card-title>
      </v-card>
    </div>
    <div>
      <game-item v-for="post in posts" :post="post" :key="post.id" @updateScore="handleChangeScore" />
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, onMounted } from 'vue';
import GameItem from '../components/TitleItem.vue';
import Navbar from '@/components/Navbar.vue';
import axios from 'axios';

export default defineComponent({
  components: {
    GameItem,
    Navbar
  },
  setup() {
    const posts: any = ref([]);
    const score = ref(0)

    const handleChangeScore = (scoreNew: number) => {
      score.value = scoreNew;
    }

    const fetchPosts = async () => {
        const categoriesId = [8, 15, 6, 20, 5]
        try {
            const categories = categoriesId.map(async categoryId => {
                return new Promise(async (resolve) => {
                    const response = await axios.get(`http://jservice.io/api/category?id=${categoryId}`);
                    resolve(response.data);
                })
            })
            Promise.all(categories).then(res => res.forEach((category: any) => {
                let newClues = category.clues.slice(0, 5)
                // let newClues = category.clues.filter((clue: { value: any; }) => clue.value == 100 || clue.value == 200 || clue.value == 300 || clue.value == 400 || clue.value == 500).slice(0, 5)

                // const uniqClues = Array.from(new Set(category.clues.filter((clue: { value: any; }) => clue.value == 100 || 200 || 300 || 400 || 500)))
                // let newClues = uniqClues.slice(0, 5)


                const newCategory = {
                    title: category.title,
                    id: category.id,
                    clues: newClues
                }
                posts.value.push(newCategory);
            }))
        } catch (e) {
            alert(e)
        }
    }

    onMounted(fetchPosts);

    return {
        posts, score, handleChangeScore
    }
  }
})
</script>

<style scoped>
.game {
  height: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-evenly;
  background-color: #2a3698;
}
</style>