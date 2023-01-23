<template>
  <div class="App">
    <navbar />
    <router-view />
  </div>
</template>

<script lang="ts">
import Navbar from '@/components/Navbar.vue'
export default {
  components: {
    Navbar
  }
}
</script>

<style>
.App {
  height: 100vh;
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  background-color: #2a3698;
  font-family: Verdana, Arial, Helvetica, sans-serif;
  font-weight: 400;
  font-size: 1rem;
  line-height: 1.5;
}
</style>
