<script setup lang="ts">

import { withDefaults, watch, ref } from 'vue'
import type { Ref } from 'vue'

const tdPrintableFontSize: Ref<string> = ref("3.7em")

interface Props {
  locale?: string
  startAtSunday?: boolean,
  fullNames?: boolean,
  month: number,
  year: number
}

const props = withDefaults(defineProps<Props>(), {
  locale: window.navigator.language,
  startAtSunday: false,
  fullNames: true
})

const calendarDate = new Date()
calendarDate.setMonth(props.month - 1)
calendarDate.setFullYear(props.year)

const currentMonthName: Ref<string> = ref(calendarDate.toLocaleDateString(props.locale, { month: props.fullNames ? 'long' : 'short' }).toUpperCase())
const currentYear: Ref<number> = ref(calendarDate.getFullYear())

watch(() => props.month, (newValue) => {
  if (newValue >= 1 && newValue <= 12) {
    refresh(newValue, props.year, props.startAtSunday)
  }
})

watch(() => props.year, (newValue) => {
  if (newValue > 0 && newValue <= 3000) {
    refresh(props.month, newValue, props.startAtSunday)
  }
})

watch(() => props.startAtSunday, (newValue) => {
  weekDayNames.value = getWeekDayNames(props.locale, newValue, props.fullNames)
  refresh(props.month, props.year, newValue)
})

function refresh(month: number, year: number, startAtSunday: boolean) {
  calendarDate.setMonth(month - 1)
  calendarDate.setFullYear(year)
  currentMonthName.value = calendarDate.toLocaleDateString(props.locale, { month: props.fullNames ? 'long' : 'short' }).toUpperCase()
  currentYear.value = calendarDate.getFullYear()
  calendar.value = getCalendar(startAtSunday, currentYear.value, calendarDate.getMonth() + 1)
}

function getWeekDayNames(locale: string = window.navigator.language, startAtSunday: boolean = false, fullName: boolean = true) {
  const weekDayNames = []
  const tmpDate = new Date()
  const currentDayOfWeek = tmpDate.getDay()
  const daysToAdd = startAtSunday ? (0 - currentDayOfWeek) : (1 - currentDayOfWeek)
  tmpDate.setDate(tmpDate.getDate() + daysToAdd)
  for (let i = 0; i < 7; i++) {
    weekDayNames.push(tmpDate.toLocaleDateString(locale, { weekday: fullName ? 'long' : 'short' }).toUpperCase())
    tmpDate.setDate(tmpDate.getDate() + 1)
  }
  return (weekDayNames)
}

const weekDayNames = ref(getWeekDayNames(props.locale, props.startAtSunday, props.fullNames))

function getCalendar(startAtSunday: boolean, currentFullYear: number, month: number) {
  const firstDayOfMonth = new Date(currentFullYear, month - 1, 1).getDay()
  const daysInMonth = new Date(currentFullYear, month, 0).getDate()

  let calendar = []
  let currentWeek = []

  const firstDayOfWeekIndex = startAtSunday ? 0 : 1

  let emptyCellsBefore = firstDayOfMonth - firstDayOfWeekIndex

  if (emptyCellsBefore < 0) {
    emptyCellsBefore += 7
  }

  for (let i = 0; i < emptyCellsBefore; i++) {
    currentWeek.push('')
  }

  for (let day = 1; day <= daysInMonth; day++) {
    currentWeek.push(day)
    if (currentWeek.length === 7) {
      calendar.push(currentWeek)
      currentWeek = []
    }
  }

  if (currentWeek.length > 0) {
    while (currentWeek.length < 7) {
      currentWeek.push('')
    }
    calendar.push(currentWeek)
  }
  tdPrintableFontSize.value = calendar.length > 5 ? "3em" : "3.7em"
  return (calendar)
}

const calendar: Ref<(string | number)[][]> = ref([])

calendar.value = getCalendar(props.startAtSunday, currentYear.value, calendarDate.getMonth() + 1)

</script>

<template>
  <h1 class="text-center"> {{ currentMonthName }} {{ currentYear }} </h1>
  <table class="table table-bordered">
    <thead>
      <tr>
        <th v-for="weekDayName in weekDayNames" :key="weekDayName"> {{ weekDayName }}</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="week in calendar">
        <td v-for="monthDayNumber in week"> {{ monthDayNumber }}</td>
      </tr>
    </tbody>
  </table>
</template>

<style>
h1 {
  text-align: center;
  margin: 0em 0em 0.5em 0em;
  font-size: 6em;
  font-weight: bolder;
}

table {
  table-layout: fixed;

}

th {
  font-size: 2em;
}

td {
  font-size: 6em;
  font-weight: bolder;
}

th,
td {
  text-align: center;
}

@media print {
  table {
    border: solid #000 !important;
    border-width: 2px 0 0 2px !important;
    border-collapse: collapse;
  }

  th {
    font-size: 1.5em;
  }

  td {
    font-size: v-bind(tdPrintableFontSize);
  }

  th,
  td {
    border: solid #000 !important;
    border-width: 0 2px 2px 0 !important;
  }
}
</style>