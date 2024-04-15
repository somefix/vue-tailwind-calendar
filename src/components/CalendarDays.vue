<script setup lang="ts">

export interface IDay {
  date: string;
  day: string;
  isToday?: boolean;
  isSelected?: boolean;
  isCurrentMonth?: boolean;
}

const props = defineProps<{
  days: IDay[];
}>()
</script>

<template>
  <div class="isolate mt-2 grid grid-cols-7 gap-px rounded-lg bg-gray-200 text-sm shadow ring-1 ring-gray-200">
    <button
      v-for="(day, dayIdx) in props.days"
      :key="day.date"
      type="button"
      :class="[
				'py-1.5 hover:bg-gray-100 focus:z-10',
				day.isCurrentMonth ? 'bg-white' : 'bg-gray-50',
				(day.isSelected || day.isToday) && 'font-semibold',
				day.isSelected && 'text-white',
				!day.isSelected && day.isCurrentMonth && !day.isToday && 'text-gray-900',
				!day.isSelected && !day.isCurrentMonth && !day.isToday && 'text-gray-400',
				day.isToday && !day.isSelected && 'text-indigo-600',
				dayIdx === 0 && 'rounded-tl-lg',
				dayIdx === 6 && 'rounded-tr-lg',
				dayIdx === days.length - 7 && 'rounded-bl-lg',
				dayIdx === days.length - 1 && 'rounded-br-lg'
      ]"
    >
      <time
        :datetime="day.date"
        :class="[
					'mx-auto flex h-7 w-7 items-center justify-center rounded-full',
					day.isSelected && day.isToday && 'bg-indigo-600',
					day.isSelected && !day.isToday && day.isCurrentMonth && 'bg-gray-900',
					day.isSelected && !day.isCurrentMonth && 'bg-gray-300',
					]">
        {{ day.day }}
      </time>
    </button>
  </div>
</template>