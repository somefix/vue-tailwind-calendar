<script setup lang="ts">
const props = defineProps<{
  weekDays: string[]
}>()
</script>

<template>
  <div class="mt-6 grid grid-cols-7 text-center text-xs leading-6 text-gray-500">
    <div v-for="day in props.weekDays" :key="day">
      {{ day }}
    </div>
  </div>
</template>
