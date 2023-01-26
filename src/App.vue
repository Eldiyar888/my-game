<template>
  <div class="App">
    <navbar />
    <router-view />
  </div>
</template>

<script lang="ts">
import Navbar from '@/components/Navbar.vue'
import { defineComponent } from 'vue';
import { RouterView } from 'vue-router';
export default defineComponent({
  components: {
    Navbar,
  }
})
</script>

<style>
.App {
  height: 100vh;
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: Verdana, Arial, Helvetica, sans-serif;
  font-weight: 400;
  font-size: 1rem;
  line-height: 1.5;
  background-color: #2a3698;
}
</style>
