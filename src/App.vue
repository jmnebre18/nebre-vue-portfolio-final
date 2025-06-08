<template>
  <div class="app">
    <router-view />
  </div>
</template>

<script setup lang="ts">
// App component now handles routing between login and portfolio
</script>

<style scoped>
.app {
  height: 100vh;
  width: 100vw;
  background-color: var(--gray-50);
  overflow: hidden;
}
</style>
