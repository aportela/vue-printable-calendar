<script setup lang="ts">

import { ref } from 'vue'
import type { Ref } from 'vue'


function getWeekDayNames(startAtSunday: boolean) {
  const weekDayNames = [];
  const currentDate = new Date();

  const currentDayOfWeek = currentDate.getDay();
  const daysToAdd = startAtSunday ? (0 - currentDayOfWeek) : (1 - currentDayOfWeek);
  currentDate.setDate(currentDate.getDate() + daysToAdd);

  for (let i = 0; i < 7; i++) {
    weekDayNames.push(currentDate.toLocaleDateString(window.navigator.language, { weekday: 'long' }).toUpperCase());
    currentDate.setDate(currentDate.getDate() + 1);
  }
  return (weekDayNames)
}

const weekDayNames = getWeekDayNames(false)

const monthName = (new Date()).toLocaleDateString(window.navigator.language, { month: 'long' }).toUpperCase()

const year = 2024

function getCalendar(startAtSunday: boolean, year: number, month: number) {
  const firstDayOfMonth = new Date(year, month - 1, 1).getDay();
  const daysInMonth = new Date(year, month, 0).getDate();

  let calendar = [];
  let currentWeek = [];

  // Determine el índice del primer día de la semana según el parámetro startAtSunday
  const firstDayOfWeekIndex = startAtSunday ? 0 : 1;

  // Determine el número de celdas vacías antes del primer día del mes
  let emptyCellsBefore = firstDayOfMonth - firstDayOfWeekIndex;
  if (emptyCellsBefore < 0) {
    emptyCellsBefore += 7;
  }

  // Celdas vacías antes del primer día del mes
  for (let i = 0; i < emptyCellsBefore; i++) {
    currentWeek.push('');
  }

  // Días del mes
  for (let day = 1; day <= daysInMonth; day++) {
    currentWeek.push(day);
    if (currentWeek.length === 7) {
      calendar.push(currentWeek);
      currentWeek = [];
    }
  }

  // Agregar la última semana si tiene días
  if (currentWeek.length > 0) {
    while (currentWeek.length < 7) {
      currentWeek.push('');
    }
    calendar.push(currentWeek);
  }

  calendar.push(currentWeek);
  return calendar;

}

const calendar = getCalendar(false, 2024, 3)
</script>

<template>
  <div class="container-xl">
    <h1 class="text-center"> {{ monthName }} {{ year }} </h1>
    <div id="calendar">
      <table class="table table-bordered">
        <thead>
          <tr>
            <th v-for="weekDayName in weekDayNames" :key="weekDayName"> {{ weekDayName }}</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="week in calendar">
            <td v-for="day in week"> {{ day }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>