<script setup lang="ts">
const props = defineProps<{
  displayedData: string;
}>()

const emit = defineEmits<{
  (e: 'clickPrev'): void;
  (e: 'clickNext'): void;
}>()</script>

<template>
  <div class="flex items-center text-center text-gray-900">
    <button @click.prevent="emit('clickPrev')" type="button" class="-m-1.5 flex flex-none items-center justify-center p-1.5 text-gray-400 hover:text-gray-500">
      <span class="sr-only">Previous month</span>
      <svg class="h-5 w-5" viewBox="0 0 20 20" fill="currentColor" aria-hidden="true">
        <path fill-rule="evenodd" d="M12.79 5.23a.75.75 0 01-.02 1.06L8.832 10l3.938 3.71a.75.75 0 11-1.04 1.08l-4.5-4.25a.75.75 0 010-1.08l4.5-4.25a.75.75 0 011.06.02z" clip-rule="evenodd" />
      </svg>
    </button>
    <div class="flex-auto text-sm font-semibold">{{ props.displayedData }}</div>
    <button @click.prevent="emit('clickNext')" type="button" class="-m-1.5 flex flex-none items-center justify-center p-1.5 text-gray-400 hover:text-gray-500">
      <span class="sr-only">Next month</span>
      <svg class="h-5 w-5" viewBox="0 0 20 20" fill="currentColor" aria-hidden="true">
        <path fill-rule="evenodd" d="M7.21 14.77a.75.75 0 01.02-1.06L11.168 10 7.23 6.29a.75.75 0 111.04-1.08l4.5 4.25a.75.75 0 010 1.08l-4.5 4.25a.75.75 0 01-1.06-.02z" clip-rule="evenodd" />
      </svg>
    </button>
  </div>
</template>