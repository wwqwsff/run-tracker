<script setup>
import { ref, computed } from 'vue'
import { BaseButton, Dropwindow, InputWithText, Title } from './index'
import { runTypes } from '@/mock-api'

const newRun = ref({
  distance: '',
  pace: '',
  time: '',
  type: '',
  notes: ''
})
const errorMessege = ref('')
const errors = ref({
  distance: false,
  pace: false,
  time: false
})
const emit = defineEmits(['addNewRun'])

const addNewRun = () => {
  errorMessege.value = ''
  errors.value = {
    distance: false,
    pace: false,
    time: false
  }
  if (!newRun.value.distance) {
    errors.value.distance = true
  }

  if (!newRun.value.pace) {
    errors.value.pace = true
  }

  if (!newRun.value.time) {
    errors.value.time = true
  }

  if (!newRun.value.distance || !newRun.value.pace || !newRun.value.time) {
    errorMessege.value = 'Пожалуйста, заполните обязательные поля'
    return
  }
  if (!/^\d{1,2}:\d{2}$/.test(newRun.value.pace)) {
    errorMessege.value = 'Укажите темп в формате ММ:СС (например 5:30)'
    return
  }

  const run = {
    id: Date.now(),
    date: new Date(),
    distance: newRun.value.distance,
    pace: newRun.value.pace,
    time: newRun.value.time,
    type: newRun.value.type ?? 'Не добавлен тип',
    notes: newRun.value.notes ?? 'Не добавлены заметки'
  }

  emit('addNewRun', run)

  newRun.value = {
    distance: '',
    pace: '',
    time: '',
    type: '',
    notes: ''
  }
}
</script>

<template>
  <div class="add-run">
    <BaseButton
      class="add-run-btn"
      form="rectangle"
      text="Добавить пробежку"
      color="yello"
      @click="addNewRun"
    />

    <div class="add-base-information">
      <div class="add-run-input">
        <InputWithText
          text2="км:"
          placeholder="0"
          v-model="newRun.distance"
          :hasError="errors.distance"
        >
        </InputWithText>

        <InputWithText
          text2="темп:"
          placeholder="0:00"
          v-model="newRun.pace"
          :hasError="errors.pace"
        >
        </InputWithText>
        <InputWithText
          class="add-run-in1"
          text2="время:"
          placeholder="00"
          v-model="newRun.time"
          :hasError="errors.time"
        >
        </InputWithText>
      </div>
      <div class="add-run-input-2">
        <Dropwindow
          variation="тип:"
          look="Выберите тип занятия"
          :options="runTypes"
          v-model="newRun.type"
        ></Dropwindow>

        <InputWithText text2="заметки:" placeholder="введите заметку" v-model="newRun.notes">
        </InputWithText>
      </div>
    </div>
    <Title
      v-if="errorMessege"
      :text="errorMessege"
      color="error-message"
      class="error-class"
    ></Title>
  </div>
</template>

<style scoped>
.info-items {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.main-content {
  margin-top: 480px;
  width: 100%;
  min-height: calc(100vh - 180px);
  box-sizing: border-box;
}
.content-wrapper {
  display: flex;
  justify-content: space-between;
  width: 100%;

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

.add-run {
  width: 625px;
  height: 260px;
  background: linear-gradient(180deg, rgba(212, 161, 122, 0.8) 0%, rgba(196, 78, 24, 0.8) 100%);
  font-family: 'Times New Roman', Times, serif;
  border-radius: 6px;
  padding: 15px;
}
.error-class {
  margin-top: -30px;
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
.history-title {
  padding-top: 10px;
  font-weight: bold;
}
.history-stast {
  display: flex;
  flex-direction: row;
  gap: 100px; /* Заменит margin-right у .add-run-input */
  padding: 20px 0 0;
  margin-top: 25px;
}
.btn-history {
  background: linear-gradient(180deg, rgba(212, 161, 122, 0.8) 0%, rgba(196, 78, 24, 0.8) 100%);
  margin-left: 70px;
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
  gap: 170px; /* Заменит margin-right у .add-run-input */
  margin-top: 15px;
  color: #3b3a3a;
}
.h-container-bt {
  display: flex;
  flex-direction: row;
  gap: 35px;
  margin-left: 700px;
  margin-top: -20px;
}

.add-base-information {
  display: flex;
  flex-direction: row;
  gap: 5px; /* Заменит margin-right у .add-run-input */
  padding: 20px 0 0;
  margin-top: 25px;
}
.add-run-border {
  width: 625px;
  height: 330px;
  border: 2px solid black;
  margin: 0 auto;
  padding: 17px 0 0 28px;
  text-align: left;
}

.add-run-input {
  margin-top: 3px;
  margin-right: 20px;
  padding: 0px 20px 0 0;
}
.add-run-in1 {
  margin-top: 27px;
}

.montName {
  font-size: 24px;
  text-align: left;
  margin: 0 0 20px 0;
  font-family: 'Times New Roman', Times, serif;
  color: #000000;
}
.add-run-btn {
  margin: 0 0 0px 0;
}
.info {
  width: 100%;
}

.add-run-input-2 {
  display: flex;
  flex-direction: column;
  gap: 2px;
  margin-bottom: 27px;
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
</style>
