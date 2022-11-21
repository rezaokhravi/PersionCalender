<script setup>
import { ref } from 'vue'

defineProps();

import moment from 'jalali-moment';
import {chunk} from 'lodash';

const startWeek = moment().locale('fa').startOf('month').week();
const endWeek = moment().locale('fa').endOf('month').week();

const endDayInMonth = moment().locale('fa').endOf('jmonth').daysInMonth();
let allDaysInMonth = Array(endDayInMonth).fill(0).map((n, i) => moment().startOf('jmonth').clone().add(n + i, 'days'));


let currentDay = allDaysInMonth[0];
const SHANBE = 0;
const JOMEE = 6;
if (currentDay.jDay() !== SHANBE) {
  do {
    currentDay = moment(currentDay).subtract(1, 'days');
    allDaysInMonth.unshift(currentDay);
  } while (currentDay.jDay() !== SHANBE)
}

currentDay = allDaysInMonth[allDaysInMonth.length - 1];
if (currentDay.jDay() !== JOMEE) {
  do {
    currentDay = moment(currentDay).add(1, 'days');
    allDaysInMonth.push(currentDay);
  } while (currentDay.jDay() !== JOMEE)
}

let calendar = ref([]);
let indexSlicing = 0;
const daysChunk = chunk(allDaysInMonth, 7);
for (let week = startWeek; week <= endWeek; week++) {

  calendar.value.push({
    week: week,
    days: daysChunk[indexSlicing]
  })
  indexSlicing++;
}
</script>

<template>
  <div class="container mx-auto bg-cyan-800">
      <div class="grid gap-4 grid-cols-7" v-for="(item, itemIndex) of calendar" :key="itemIndex">
        <div class="w-full h-full" v-for="(day, dayIndex) of item.days" :key="dayIndex">
          <span>{{day.locale("fa").format("YYYY/MM/DD")}}</span>
        </div>
      </div>
  </div>
</template>

<style scoped>

</style>
