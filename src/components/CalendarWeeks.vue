<script setup lang="ts">
const props = defineProps<{
  weeks: string[];
}>()
</script>

<template>
  <div class="mt-6 grid grid-cols-7 text-center text-xs leading-6 text-gray-500">
    <div v-for="week in props.weeks" :key="week">{{ week }}</div>
  </div>
</template>

<style scoped>

</style>