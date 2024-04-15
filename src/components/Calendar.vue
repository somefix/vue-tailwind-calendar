<script setup lang="ts">
import type { Dayjs } from 'dayjs';
import 'dayjs/locale/ru'
import { toRefs } from "vue";
import ConnectedCalendarDays from '@/components/ConnectedCalendarDays.vue'

const props = defineProps<{
  selectedDate: Dayjs;
	displayedMonth: Dayjs;
}>()

const { selectedDate, displayedMonth } = toRefs(props);
</script>

<template>
  <connected-calendar-days :displayed-month="displayedMonth" :selected-date="selectedDate" />
</template>
