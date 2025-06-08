<template>
  <div class="portfolio-layout">
    <Sidebar />
    <main class="main-content">
      <router-view />
    </main>
  </div>
</template>

<script setup lang="ts">
import Sidebar from '../components/Sidebar.vue'
</script>

<style scoped>
.portfolio-layout {
  display: flex;
  height: 100vh;
  width: 100vw;
  background-color: var(--gray-50);
  overflow: hidden;
}

.main-content {
  flex: 1;
  overflow-y: auto;
  background-color: var(--gray-50);
  width: 100%;
}

@media (max-width: 768px) {
  .portfolio-layout {
    flex-direction: column;
    height: auto;
    min-height: 100vh;
  }
  
  .main-content {
    min-height: calc(100vh - 80px);
  }
}
</style>