<template>
  <div class="game">
    <game-item v-for="post in posts" :post="post" :key="post.id"/>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import axios from 'axios';
import GameItem from '../components/TitleItem.vue';

export default defineComponent({
    components: {
      GameItem
    },
    data() {
      return {
        posts: <any[]> []
      }
    },
    methods: {
      async fetchPosts() {
        const categoriesId = [8, 15, 6, 20, 5]
            try {
                const categories = categoriesId.map(async categoryId => {
                  return new Promise(async (resolve) => {
                    const response = await axios.get(`http://jservice.io/api/category?id=${categoryId}`);
                    resolve(response.data);
                  })
                })
                Promise.all(categories).then(res => res.forEach((category: any, categoryIndex) => {
                  let newClues = category.clues.slice(0, 5)
                  // let newClues =category.clues.filter((clue: { value: any; }) => clue.value == 100 || clue.value == 200 || clue.value == 300 || clue.value == 400 || clue.value == 500).slice(0, 5)

                  // const uniqClues = Array.from(new Set(category.clues.filter((clue: { value: any; }) => clue.value == 100 || 200 || 300 || 400 || 500)))
                  // let newClues = uniqClues.slice(0, 5)

                  console.log(newClues)

                  let newCategory = {
                    title: category.title,
                    id: category.id,
                    clues: newClues
                  }
                  this.posts.push(newCategory);
                  console.log(this.posts)
                }))
            } catch (e) {
                alert(e)
            }
        },
    },
    mounted() {
      this.fetchPosts();
    }
  })
</script>

<style scoped>
  .game {
    height: calc(100% - 100px);
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
  }
</style>