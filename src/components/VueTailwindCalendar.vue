<script setup lang="ts">
import dayjs, { Dayjs } from 'dayjs'
import 'dayjs/locale/ru'
import localeData from 'dayjs/plugin/localeData'
import weekday from 'dayjs/plugin/weekday'
import { reactive, ref, toRefs, unref } from 'vue'
// import VtcCalendar from "@/components/Calendar.vue";
import CalendarHeader from '@/components/CalendarHeader.vue'
import CalendarWeeks from '@/components/CalendarWeeks.vue'
import ConnectedCalendarDays from '@/components/ConnectedCalendarDays.vue'
import updateLocale from 'dayjs/plugin/updateLocale'

// const VISIBLE_DATES_COUNT = 42;

dayjs.extend(localeData)
dayjs.extend(weekday)
// dayjs.extend(updateLocale)
dayjs.locale('ru')
dayjs.updateLocale('ru', {
  name: 'ru',
  weekStart: 1,
});

const weekDays = dayjs.weekdaysMin()
interface ICalendar {
  selectedDate: Dayjs
  displayedMonth: Dayjs
}
const calendar: ICalendar = reactive({
  selectedDate: dayjs().add(1, 'month').date(1),
  displayedMonth: dayjs()
})

function next(calendar: ICalendar): void {
  calendar.displayedMonth = calendar.displayedMonth.add(1, 'month')
  // calendar.selectedDate = calendar.displayedMonth.date(1)
}

function previous(calendar: ICalendar): void {
  calendar.displayedMonth = calendar.displayedMonth.subtract(1, 'month')
  // calendar.selectedDate = calendar.displayedMonth.date(1)
}
</script>

<template>
  <div class="hidden w-1/2 max-w-md flex-none border-l border-gray-100 px-8 py-10 md:block">
    <calendar-header
      :month="calendar.displayedMonth.format('MMMM')"
      :year="calendar.displayedMonth.format('YYYY')"
      @click:prev="previous(calendar)"
      @click:next="next(calendar)"
    />

    <calendar-weeks :week-days="weekDays" />

    <connected-calendar-days
      :displayed-month="calendar.displayedMonth"
      :selected-date="calendar.selectedDate"
    />
  </div>
</template>

<style scoped></style>
