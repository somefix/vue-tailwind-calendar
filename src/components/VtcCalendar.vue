<script setup lang="ts">

// defineProps<{
//   calendar: {
//     date: () => any[]
//     month: string
//     year: number
//     years: () => number[]
//     onPrevious: () => void
//     onNext: () => void
//     onPreviousYear: () => void
//     onNextYear: () => void
//     openMonth: () => void
//     setMonth: ($event: number) => void
//     openYear: () => void
//     setYear: ($event: number) => void
//   }
//   weeks: string[]
//   weekNumber: boolean
//   asRange: boolean
// }>()
</script>

<template>
  <div class="isolate mt-2 grid grid-cols-7 gap-px rounded-lg bg-gray-200 text-sm shadow ring-1 ring-gray-200">
    <button v-for="(day, dayIdx) in days" :key="day.date" type="button" :class="['py-1.5 hover:bg-gray-100 focus:z-10', day.isCurrentMonth ? 'bg-white' : 'bg-gray-50', (day.isSelected || day.isToday) && 'font-semibold', day.isSelected && 'text-white', !day.isSelected && day.isCurrentMonth && !day.isToday && 'text-gray-900', !day.isSelected && !day.isCurrentMonth && !day.isToday && 'text-gray-400', day.isToday && !day.isSelected && 'text-indigo-600', dayIdx === 0 && 'rounded-tl-lg', dayIdx === 6 && 'rounded-tr-lg', dayIdx === days.length - 7 && 'rounded-bl-lg', dayIdx === days.length - 1 && 'rounded-br-lg']">
      <time :datetime="day.date" :class="['mx-auto flex h-7 w-7 items-center justify-center rounded-full', day.isSelected && day.isToday && 'bg-indigo-600', day.isSelected && !day.isToday && 'bg-gray-900']">{{ day.date.split('-').pop().replace(/^0/, '') }}</time>
    </button>
  </div>
</template>