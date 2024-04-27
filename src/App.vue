<script setup lang="ts">
import TaskFlowBoard from './components/TaskFlowBoard.vue'
</script>

<template>
  <div class="p-10 h-screen bg-[#1d2125] overflow-auto">
    <h1 class="text-4xl text-white flex items-center mb-10">
      <img class="mr-3 w-10 aspect-square" src="/favicon.svg" alt="Task Flow" />
      Task Flow
    </h1>
    <TaskFlowBoard />
  </div>
</template>
