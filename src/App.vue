<script setup lang="ts">
import GameComp from "./components/GameComp.vue";
</script>

<template>
  <div class="app">
    <GameComp :quantity="4"/>
  </div>
</template>

<style scoped lang="scss">
.app{
  min-height: 100vh;
  background-color: #1a1a1a;
  display: flex;
  align-items: center;
}
</style>
