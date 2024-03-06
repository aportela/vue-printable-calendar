<script setup lang="ts">
import { ref } from 'vue'
import type { Ref } from 'vue'
import PrintableCalendar from './components/PrintableCalendar.vue'
import './style.css'

const startAtSunday: Ref<boolean> = ref(false)

const currentDate = new Date()

const selectedMonthIndex: Ref<number> = ref(currentDate.getMonth() + 1)

const selectedYear: Ref<number> = ref(currentDate.getFullYear())

function onPrint() {
  window.print()
}
</script>

<template>
  <div class="d-grid gap-2 col-3 mx-auto m-2 d-print-none">
    <div class="input-group mb-3">
      <span class="input-group-text" id="basic-addon1">Week start at</span>
      <select class="form-control" placeholder="Week day starts at" aria-label="Month" aria-describedby="month selector"
        v-model.boolean="startAtSunday">
        <option :value="false">Monday</option>
        <option :value="true">Sunday</option>
      </select>
      <span class="input-group-text" id="basic-addon1">Month</span>
      <select class="form-control text-end" placeholder="Select month" aria-label="Month"
        aria-describedby="month selector" v-model.number="selectedMonthIndex">
        <option v-for="index in 12" :key="index" :value="index">{{ index }}</option>
      </select>
      <span class="input-group-text" id="basic-addon2">Year</span>
      <input type="number" min="1" max="3000" class="form-control text-end" placeholder="Select year" aria-label="Year"
        aria-describedby="year selector" v-model.number="selectedYear">
      <button type="button" class="btn btn-lg btn-primary" @click.prevent="onPrint">🖶</button>
    </div>
  </div>
  <PrintableCalendar :start-at-sunday="startAtSunday" :full-names="true" :month="selectedMonthIndex"
    :year="selectedYear" />
</template>

<style scoped></style>