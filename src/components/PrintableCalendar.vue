<script setup lang="ts">

import { withDefaults } from 'vue'

interface Props {
  locale?: string
  startAtSunday?: boolean,
  fullNames?: boolean
}

const props = withDefaults(defineProps<Props>(), {
  locale: window.navigator.language,
  startAtSunday: false,
  fullNames: true
})

const currentDate = new Date()
const currentMonthName = currentDate.toLocaleDateString(props.locale, { month: props.fullNames ? 'long' : 'short' }).toUpperCase()
const currentYear = currentDate.getFullYear()

function getWeekDayNames(locale: string = window.navigator.language, startAtSunday: boolean = false, fullName: boolean = true) {
  const weekDayNames = []
  const currentDate = new Date()
  const currentDayOfWeek = currentDate.getDay()
  const daysToAdd = startAtSunday ? (0 - currentDayOfWeek) : (1 - currentDayOfWeek)
  currentDate.setDate(currentDate.getDate() + daysToAdd)
  for (let i = 0; i < 7; i++) {
    weekDayNames.push(currentDate.toLocaleDateString(locale, { weekday: fullName ? 'long' : 'short' }).toUpperCase())
    currentDate.setDate(currentDate.getDate() + 1)
  }
  return (weekDayNames)
}

const weekDayNames = getWeekDayNames(props.locale, props.startAtSunday, props.fullNames)

function getCalendar(startAtSunday: boolean, currentFullYear: number, month: number) {
  const firstDayOfMonth = new Date(currentFullYear, month - 1, 1).getDay()
  const daysInMonth = new Date(currentFullYear, month, 0).getDate()

  let calendar = []
  let currentWeek = []

  const firstDayOfWeekIndex = startAtSunday ? 0 : 1

  let emptyCellsBefore = firstDayOfMonth - firstDayOfWeekIndex;
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
  calendar.push(currentWeek)
  return (calendar)
}

const calendar = getCalendar(props.startAtSunday, currentYear, currentDate.getMonth() + 1)

</script>

<template>
  <h1 class="text-center"> {{ currentMonthName }} {{ currentYear }} </h1>
  <div id="calendar">
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
  </div>
</template>