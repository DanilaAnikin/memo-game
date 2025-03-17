<template>
  <div class="min-h-screen flex flex-col gap-5 justify-center items-center bg-gray-200">
    <p class="text-4xl">Memo Game</p>
    <MemoryGame />
  </div>
</template>

<script setup lang="ts">
import MemoryGame from './components/MemoryGame.vue';
</script>