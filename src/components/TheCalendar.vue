<template>
  <div class="calendar">
    <header class="calendar__header">{{today.format('MMMM YYYY')}}</header>
    <div v-for="day in weeks[0]" class="calendar__cell disabled">{{day.format('dd')}}</div>
    <template v-for="week in weeks" class="calendar__row">
      <div
          v-for="day in week"
          :class="[
              'calendar__cell',
              day.isSame(today, 'month') ? 'active': 'disabled',
              day.isSame(today, 'day') ? 'today' : 'day'
          ]"
      >
        {{day.format('D')}}
      </div>
    </template>
  </div>
</template>

<script setup>
import {ref, onBeforeMount} from 'vue'
import moment from "moment";

const today = ref(moment());
const weeks = []

onBeforeMount(() => {
  const numOfColumns = 7;
  const numOfRows = 6;

  const monthFirstMonday = moment().startOf('month').weekday(1);
  const lastDayInCalendar = monthFirstMonday.clone().add((numOfColumns * numOfRows) - 1, 'days');

  let loopCurrentDay = monthFirstMonday.clone();
  let loopCurrentWeek = [];

  while (loopCurrentDay.isSameOrBefore(lastDayInCalendar, 'day')){
    loopCurrentWeek.push(loopCurrentDay.clone());

    if(loopCurrentWeek.length === numOfColumns){
      weeks.push(loopCurrentWeek);
      loopCurrentWeek = [];
    }

    loopCurrentDay = loopCurrentDay.add(1, 'days');
  }
})
</script>

<style scoped lang="sass">
.calendar
  display: grid
  grid-template-columns: repeat(7, 1fr)
  gap: 2px
  color: white

  &__header
    width: 100%
    grid-column: 1 / 8
    font-size: 36px
    text-align: center
    padding: 40px 0

  &__cell
    padding: 20px
    text-align: center
    font-size: 24px

.active
  color: #fff

.disabled
  color: grey

.today
  background-color: rgba(50, 50, 255, 0.5)

.day:hover
  background-color: rgba(100, 100, 255, 0.2)

</style>