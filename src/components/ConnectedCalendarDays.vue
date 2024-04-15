<script setup lang="ts">
import dayjs, { type Dayjs } from 'dayjs'
import { computed, reactive } from 'vue'
import CalendarDays from '@/components/CalendarDays.vue'
import weekday from 'dayjs/plugin/weekday'
import isToday from 'dayjs/plugin/isToday'
import isSameOrAfter from 'dayjs/plugin/isSameOrAfter'
import isSameOrBefore from 'dayjs/plugin/isSameOrBefore'

const VISIBLE_DATES_COUNT = 42;

dayjs.extend(weekday)
dayjs.extend(isToday)
dayjs.extend(isSameOrAfter)
dayjs.extend(isSameOrBefore)
dayjs.locale('ru')

const props = defineProps<{
  selectedDate: Dayjs;
  displayedMonth: Dayjs;
}>()

const firstDateOfMonth = computed(() => props.displayedMonth.date(1))
const lastDayOfMonth = computed(() =>
  props.displayedMonth.date(
    props.displayedMonth.daysInMonth()
  )
);

function getPreviousDays(displayedMonth: Dayjs) {
  const firstDateOfMonth = displayedMonth.date(1)
  const weekDayNumberOfFirstDate = firstDateOfMonth.weekday()
  const previousDatesOfMonth = []

  for (let i = 0; i < weekDayNumberOfFirstDate; i++) {
    previousDatesOfMonth.push(firstDateOfMonth.weekday(i))
  }

  return previousDatesOfMonth
}

function getCurrentDays(displayedMonth: Dayjs) {
  const datesOfMonth = Array.from(
    { length: displayedMonth.daysInMonth() },
    (v, idx) => displayedMonth.date(idx + 1),
  )

  return datesOfMonth
}

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

function isCurrentMonth(date: Dayjs): boolean {
  return date.isSameOrAfter(firstDateOfMonth.value) && date.isSameOrBefore(lastDayOfMonth.value);
}

function isSelected(date: Dayjs): boolean {
  return date.isSame(props.selectedDate, 'day');
}

const days = computed(() => {
  const days = [...getPreviousDays(props.displayedMonth), ...getCurrentDays(props.displayedMonth), ...getNextDays(props.displayedMonth)]

  return days.map((day: Dayjs) => ({
    date: day.format(),
    day: day.format('DD'),
    isToday: day.isToday(),
    isSelected: isSelected(day),
    isCurrentMonth: isCurrentMonth(day),
  }))
});
</script>

<template>
  <calendar-days :days="days" />
</template>