<script setup lang="ts">
import PrintableCalendar from './components/PrintableCalendar.vue'
import './style.css'
</script>

<template>
  <PrintableCalendar :start-at-sunday="false" :full-names="true" />
</template>

<style scoped></style>