<script setup>
import { computed, ref } from 'vue'
import {
  AddRunWindow,
  BaseButton,
  Dropwindow,
  InputWithText,
  RunDataItem,
  Title,
  RunMainItem
} from './components'
import { monthName, monthNameNumber, dayHumber, dayName, runTypes } from './mock-api'

const nowmonth = computed(() => {
  const now = new Date()
  return {
    month: monthName[now.getMonth()],
    monthNumber: monthNameNumber[now.getMonth()],
    dayNumber: dayHumber[now.getDate() - 1],
    dayName: dayName[now.getDay()],
    date: now
  }
})

const deleteRun = id => {
  runs.value = runs.value.filter(run => run.id !== id)
}

const sortOptions = ref({
  type: '',
  month: ''
})

const runs = ref([])

const tempMonth = computed(() => {
  if (runs.value.length == 0) return '0:00'
  const totalPace = runs.value.reduce((sum, run) => {
    const [min, sec] = run.pace.split(':').map(Number)
    return sum + min + sec / 60
  }, 0)
  const avg = totalPace / runs.value.length
  const min = Math.floor(avg)
  const sec = Math.round((avg - min) * 60)
  return `${min}:${sec.toString().padStart(2, '0')}`
})

const kmMonth = computed(() => {
  return runs.value.reduce((sum, run) => sum + parseFloat(run.distance || 0), 0).toFixed(1)
})

const trainingMonth = computed(() => {
  return runs.value.length
})

const infoItems = computed(() => {
  return [
    { quality: 'км', value: kmMonth.value },
    { quality: 'темп', value: tempMonth.value },
    { quality: 'тренировок', value: trainingMonth.value }
  ]
})

const addNewRun = run => {
  runs.value.unshift({
    ...run,
    id: Date.now(),
    date: new Date()
  })
}
</script>

<template>
  <header class="header">
    <Title text="Беговой трекер" size="big" class="title" />

    <div class="content-wrapper">
      <div class="info-items">
        <h2 class="montName">{{ nowmonth.month }}</h2>
        <RunDataItem
          v-for="item in infoItems"
          :key="item.quality"
          :quality="item.quality"
          :value="item.value"
        />
      </div>

      <AddRunWindow @addNewRun="addNewRun" />
    </div>
  </header>
  <main class="main-content">
    <RunMainItem
      :runs="runs"
      :nowmonth="nowmonth"
      :initial-sort-options="sortOptions"
      @update:sort-options="newOpts => (sortOptions = newOpts)"
      @delete-run="deleteRun"
    />
  </main>
</template>

<style>
html,
body {
  margin: 0;
  padding: 0;
  min-height: 100vh;
  width: 100%;
}

body {
  background: linear-gradient(180deg, rgba(212, 161, 122, 0.8) 0%, rgba(196, 78, 24, 0.8) 100%);
  background-attachment: fixed;
  position: relative;
}
</style>
<style scoped>
.info-items {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.header {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  flex-direction: column;
  align-items: flex-start;
  padding: 1px 20px;
  box-sizing: border-box;
  z-index: 100;
  height: 180px;
}
.main-content {
  margin-top: 480px;
  width: 100%;
  min-height: calc(100vh - 180px);
  box-sizing: border-box;
}
.content-wrapper {
  display: flex;
  flex-direction: row;
  justify-content: center;
  gap: 100px;
  align-items: left;

  height: auto;
}

.title {
  margin: 0;
  text-align: center;
  width: 100%;
  font-weight: bold;
}

.month {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  margin: 0;
  position: relative;
  font-family: 'Times New Roman', Times, serif;
}

.history-run {
  width: 1041px;
  min-height: 350px;
  background-color: #f0e68c;
  margin: 0 auto;
  position: relative;
  border-radius: 6px;
  font-family: 'Times New Roman', Times, serif;
}

.btn-history {
  background: linear-gradient(180deg, rgba(212, 161, 122, 0.8) 0%, rgba(196, 78, 24, 0.8) 100%);
  margin-left: 70px;
}

.add-base-information {
  display: flex;
  flex-direction: row;
  gap: 5px;
  padding: 20px 0 0;
  margin-top: 25px;
}

.montName {
  font-size: 24px;
  text-align: center;
  margin: 0 0 20px 0;
  font-family: 'Times New Roman', Times, serif;
  color: #000000;
}

.info {
  width: 100%;
}

ul {
  width: 420px;
  height: 190px;
  list-style-type: none;
  padding: 0;
  margin: 0;
  font-size: 24px;
  color: black;
  text-align: left;
  font-family: 'Times New Roman', Times, serif;
}

@media (max-width: 768px) {
  .title {
    font-size: 32px;
  }

  .montName,
  ul {
    font-size: 20px;
  }
}

@media (max-width: 480px) {
  .title {
    font-size: 28px;
  }

  .montName,
  ul {
    font-size: 18px;
  }

  header {
    padding: 1px 15px;
  }
}
li {
  margin-bottom: 20px;
  width: 185px;
  height: 65px;
  background: linear-gradient(180deg, rgba(212, 161, 122, 0.8) 0%, rgba(196, 78, 24, 0.8) 100%);
  display: flex;
  align-items: center;
  border-radius: 6px;
}
</style>
