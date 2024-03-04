<script setup lang="ts">

import { ref } from 'vue'
import type { Ref } from 'vue'

const weekDayNames = ["Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday"]

const monthName = "March"

const year = 2024

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
      </table>
    </div>
  </div>
</template>

<style scoped>
table {
  table-layout: fixed;
}

th {
  text-align: center;
  font-size: 2em;
}

td {
  text-align: center;
  font-size: 6em;
  font-weight: bolder;
  width: 10em;
}

@media print {
  table {
    border: solid #000 !important;
    border-width: 2px 0 0 2px !important;
  }

  th,
  td {
    border: solid #000 !important;
    border-width: 0 2px 2px 0 !important;
  }
}
</style>