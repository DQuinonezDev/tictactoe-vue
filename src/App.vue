<template>
  <div class="page-container">
    <AppHeader />
    <Game />
    <AppFooter />
  </div>
</template>

<script setup>
import './assets/global.css'
import AppFooter from './components/AppFooter.vue';
import AppHeader from './components/AppHeader.vue';
import Game from './components/Game.vue';
</script>

<style scoped>
body, html {
  margin: 0;
  padding: 0;
  height: 100%;
}

.page-container {
  display: flex;
  flex-direction: column;
  min-height: 97vh;
}

footer {
  text-align: center;
  margin-top: auto;
}
</style>
