<script setup>
import { ref, computed } from 'vue'
import { BaseButton, Dropwindow, InputWithText, Title } from './index'
import { runTypes, monthName } from '@/mock-api'

const props = defineProps({
  runs: {
    type: Array,
    required: true,
    default: () => []
  },
  nowmonth: {
    type: Object,
    required: true
  },
  initialSortOptions: {
    type: Object,
    default: () => ({ type: '', month: '' })
  }
})
const sortOptions = ref({ ...props.initialSortOptions })
const sortedRuns = computed(() => {
  let result = [...props.runs]

  if (sortOptions.value.type) {
    result = result.filter(run => run.type === sortOptions.value.type)
  }

  if (sortOptions.value.month) {
    result = result.filter(run => {
      const runDate = new Date(run.date)
      return runDate.getMonth() + 1 === parseInt(sortOptions.value.month)
    })
  }

  return result
})
const emit = defineEmits(['update:sort-options', 'delete-run'])
const getRunDetails = run => [
  { value: run.time, text: null },
  { value: run.pace, text: ' мин/км' },
  { value: run.type || 'Тип не выбран', text: null }
]
</script>

<template>
  <div class="history-run">
    <Title text="История пробежек" class="history-title"></Title>

    <div class="history-stast">
      <Dropwindow
        variation="Сортировать по типу:"
        look="Все типы"
        :options="runTypes"
        sizebig="big"
        v-model="sortOptions.type"
      ></Dropwindow>

      <Dropwindow
        variation="Cортировать по месяцу:"
        look="Все месяцы"
        :options="monthName"
        v-model="sortOptions.month"
        sizebig="big"
      >
      </Dropwindow>
    </div>
    <div class="history-container" v-for="run in sortedRuns" :key="run.id">
      <div class="h-container-up">
        <div class="h-container-up-date">
          {{ nowmonth.dayNumber }}.{{ nowmonth.monthNumber }}
          {{ nowmonth.dayName }}
        </div>
        <div class="h-container-up-notes">{{ run.notes }}</div>
      </div>
      <div class="h-container-distance">{{ run.distance }} км</div>
      <div class="h-container-down">
        <template v-for="(item, index) in getRunDetails(run)" :key="index">
          <div>
            {{ item.value }}<span v-if="item.text">{{ item.text }}</span>
          </div>
        </template>
      </div>

      <BaseButton
        class="h-container-bt"
        form="very-small"
        text="Удалить"
        color="yello"
        @click="emit('delete-run', run.id)"
      />
    </div>
  </div>
</template>

<style scoped>
.history-run {
  width: 1041px;
  min-height: 350px;
  background-color: #f0e68c;
  margin: 0 auto;
  position: relative;
  border-radius: 6px;
  font-family: 'Times New Roman', Times, serif;
}
.history-stast {
  display: flex;
  flex-direction: row;
  gap: 100px;
  padding: 20px 0 0;
  margin-top: 25px;
}
.history-title {
  padding-top: 10px;
  font-weight: bold;
}
.history-container {
  height: 115px;
  width: 965px;
  background: linear-gradient(180deg, rgba(212, 161, 122, 0.8) 0%, rgba(196, 78, 24, 0.8) 100%);
  position: relative;
  margin-left: 35px;
  margin-top: 10px;
  padding: 10px;
  border-radius: 6px;
  font-family: 'Times New Roman', Times, serif;
}

.h-container-up {
  display: flex;
  flex-direction: row;
  gap: 700px;
}
.h-container-up-date {
  height: 17px;
  width: 100px;
  font-size: 15px;
  text-align: center;
  margin: 0 0 20px 0;
  font-family: 'Times New Roman', Times, serif;
  color: #000000;
}
.h-container-up-notes {
  height: 45px;
  width: 285px;
  background-color: #f0e68c;
  border-radius: 6px;
  border: 1px solid black;
  padding-left: 10px;
  color: #000000;
  text-align: left;
}
.h-container-distance {
  height: 35px;
  width: 130px;
  text-align: center;
  color: #000000;
  margin-left: 200px;
  font-size: 20px;
  font-weight: bold;
}
.h-container-down {
  display: flex;
  flex-direction: row;
  gap: 230px;
  margin-top: 15px;
  color: #3b3a3a;
}
.h-container-bt {
  display: flex;
  padding-left: 35px;
  margin-left: 830px;
  margin-top: -20px;
}
</style>
