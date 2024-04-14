<script setup lang="ts">
import dayjs from 'dayjs';
import type { Dayjs } from 'dayjs';
import 'dayjs/locale/ru'
import weekday from 'dayjs/plugin/weekday'
import isToday from 'dayjs/plugin/isToday'
import isSameOrAfter from 'dayjs/plugin/isSameOrAfter'
import isSameOrBefore from 'dayjs/plugin/isSameOrBefore'
import {computed, reactive, ref, toRefs} from "vue";

dayjs.extend(weekday)
dayjs.extend(isToday)
dayjs.extend(isSameOrAfter)
dayjs.extend(isSameOrBefore)
dayjs.locale('ru')
const VISIBLE_DATES_COUNT = 42;

const props = defineProps<{
  selectedDate: Dayjs;
	displayedMonth: Dayjs;
}>()

// const { selectedDate, displayedMonth } = toRefs(props);

// const date = displayedMonth.value;

/** ====================== */
const firstDateOfMonth = computed(() => props.displayedMonth.date(1))
const lastDayOfMonth = computed(() =>
  props.displayedMonth.date(
		props.displayedMonth.daysInMonth()
  )
);
// const weekDayNumberOfFirstDate = firstDateOfMonth.weekday()
// const previousDatesOfMonth = reactive([])
// // console.log(firstDateOfMonth.weekday(0))
//
// for (let i = 0; i < weekDayNumberOfFirstDate; i++) {
// 	previousDatesOfMonth.push(firstDateOfMonth.weekday(i))
// }

function getPreviousDays(displayedMonth: Dayjs) {
	const firstDateOfMonth = displayedMonth.date(1)
	const weekDayNumberOfFirstDate = firstDateOfMonth.weekday()
	const previousDatesOfMonth = reactive([])

	for (let i = 0; i < weekDayNumberOfFirstDate; i++) {
		previousDatesOfMonth.push(firstDateOfMonth.weekday(i))
	}

	return previousDatesOfMonth
}
// console.log(previousDatesOfMonth)
/** ====================== */
// const datesOfMonth = reactive(Array.from(
// 	{ length: date.daysInMonth() },
// 	(v, idx) => date.date(idx + 1),
// ))
function getCurrentDays(displayedMonth: Dayjs) {
	const datesOfMonth = Array.from(
		{ length: displayedMonth.daysInMonth() },
		(v, idx) => displayedMonth.date(idx + 1),
	)

	return datesOfMonth
}
// console.log(datesOfMonth)
/** ====================== */
// const nextMonthOfMonth = date.add(1, 'month')
// const nextDatesOfMonth = reactive([])
//
// for (let i = 0; i < VISIBLE_DATES_COUNT - weekDayNumberOfFirstDate - date.daysInMonth(); i++ ) {
// 	nextDatesOfMonth.push(nextMonthOfMonth.date(i + 1))
// }

function getNextDays(displayedMonth: Dayjs) {
	const firstDateOfMonth = displayedMonth.date(1)
	const weekDayNumberOfFirstDate = firstDateOfMonth.weekday()
	const nextMonthOfMonth = displayedMonth.add(1, 'month')
	const nextDatesOfMonth = []

	for (let i = 0; i < VISIBLE_DATES_COUNT - weekDayNumberOfFirstDate - displayedMonth.daysInMonth(); i++ ) {
		nextDatesOfMonth.push(nextMonthOfMonth.date(i + 1))
	}

	return nextDatesOfMonth
}
// console.log(nextDatesOfMonth)
const days = computed(() => {
	return [...getPreviousDays(props.displayedMonth), ...getCurrentDays(props.displayedMonth), ...getNextDays(props.displayedMonth)]
});
// .map((date) => date.format());
// console.warn(days);


function isCurrentMonth(date: Dayjs): boolean {
	return date.isSameOrAfter(firstDateOfMonth.value) && date.isSameOrBefore(lastDayOfMonth.value);
}
function isSelected(date: Dayjs): boolean {
	return date.isSame(props.selectedDate, 'day');
}
</script>

<template>
  <div class="isolate mt-2 grid grid-cols-7 gap-px rounded-lg bg-gray-200 text-sm shadow ring-1 ring-gray-200">
    <button
      v-for="(day, dayIdx) in days"
      :key="day"
      type="button"
      :class="[
				'py-1.5 hover:bg-gray-100 focus:z-10',
				isCurrentMonth(day) ? 'bg-white' : 'bg-gray-50',
				(isSelected(day) || day.isToday()) && 'font-semibold',
				isSelected(day) && 'text-white',
				!isSelected(day) && isCurrentMonth(day) && !day.isToday() && 'text-gray-900',
				!isSelected(day) && !isCurrentMonth(day) && !day.isToday() && 'text-gray-400',
				day.isToday() && !isSelected(day) && 'text-indigo-600',
				dayIdx === 0 && 'rounded-tl-lg',
				dayIdx === 6 && 'rounded-tr-lg',
				dayIdx === days.length - 7 && 'rounded-bl-lg',
				dayIdx === days.length - 1 && 'rounded-br-lg'
      ]"
    >
      <time
        :datetime="day"
        :class="[
					'mx-auto flex h-7 w-7 items-center justify-center rounded-full',
					isSelected(day) && day.isToday() && 'bg-indigo-600',
					isSelected(day) && !day.isToday() && isCurrentMonth(day) && 'bg-gray-900',
					isSelected(day) && !isCurrentMonth(day) && 'bg-gray-300',
					]">
        {{ day.format('DD') }}
      </time>
    </button>
  </div>
</template>
