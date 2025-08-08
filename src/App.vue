<script setup>
import { computed, ref } from "vue";
import BaseButton from "./components/BaseButton.vue";
import Title from "./components/Title.vue";
import AddRunBlock from "./components/AddRunBlock.vue";
import Dropwindow from "./components/Dropwindow.vue";

const monthName = [
  "Январь",
  "Февраль",
  "Март",
  "Апрель",
  "Май",
  "Июнь",
  "Июль",
  "Август",
  "Сентябрь",
  "Октябрь",
  "Ноябрь",
  "Декабрь",
];
const monthNameNumber = [
  "01",
  "02",
  "03",
  "04",
  "05",
  "06",
  "07",
  "09",
  "10",
  "11",
  "12",
];
const dayHumber = [
  "01",
  "02",
  "03",
  "04",
  "05",
  "06",
  "07",
  "08",
  "09",
  "10",
  "11",
  "12",
  "13",
  "14",
  "15",
  "16",
  "17",
  "18",
  "19",
  "20",
  "21",
  "22",
  "23",
  "24",
  "25",
  "26",
  "27",
  "28",
  "29",
  "30",
  "31",
];
const dayName = ["Вс", "Пн", "Вт", "Ср", "Чт", "Пт", "Сб"];
const nowmonth = computed(() => {
  const now = new Date();
  return {
    month: monthName[now.getMonth()],
    monthNumber: monthNameNumber[now.getMonth()],
    dayNumber: dayHumber[now.getDate() - 1],
    dayName: dayName[now.getDay()],
    date: now,
  };
});
const deleteRun = (id) => {
  runs.value = runs.value.filter((run) => run.id !== id);
};

const sortOptions = ref({
  type: "",
  month: "",
});
const sortedRuns = computed(() => {
  let result = [...runs.value];

  if (sortOptions.value.type) {
    result = result.filter((run) => run.type === sortOptions.value.type);
  }

  if (sortOptions.value.month) {
    result = result.filter((run) => {
      const runDate = new Date(run.date);
      return runDate.getMonth() + 1 === parseInt(sortOptions.value.month);
    });
  }

  return result;
});
const runs = ref([]);
const newRun = ref({
  distance: "",
  pace: "",
  time: "",
  type: "",
  notes: "",
});
const addNewRun = () => {
  if (!newRun.value.distance || !newRun.value.pace || !newRun.value.time) {
    alert("Пожалуйста, заполните обязательные поля: км, темп и время");
    return;
  }
  const run = {
    id: Date.now(),
    date: new Date(),
    distance: newRun.value.distance,
    pace: newRun.value.pace,
    time: newRun.value.time,
    type: newRun.value.type || "Не добавлен тип",
    notes: newRun.value.notes || "Не добавлены заметки",
  };
  runs.value.unshift(run);
  newRun.value = {
    distance: "",
    pace: "",
    time: "",
    type: "",
    notes: "",
  };
};
const tempMonth = computed(() => {
  if (runs.value.length == 0) return "0:00";
  const totalPace = runs.value.reduce((sum, run) => {
    const [min, sec] = run.pace.split(":").map(Number);
    return sum + min + sec / 60;
  }, 0);
  const avg = totalPace / runs.value.length;
  const min = Math.floor(avg);
  const sec = Math.round((avg - min) * 60);
  return `${min}:${sec.toString().padStart(2, "0")}`;
});
const kmMonth = computed(() => {
  return runs.value
    .reduce((sum, run) => sum + parseFloat(run.distance || 0), 0)
    .toFixed(1);
});
const trainingMonth = computed(() => {
  return runs.value.length;
});
const kmrunning = computed(() => {
  return 5.1;
});
</script>

<template>
  <header class="header">
    <div class="title">
      <Title text1="Беговой трекер" size="big"></Title>
    </div>

    <div class="content-wrapper">
      <div class="month">
        <h2 class="montName">{{ nowmonth.month }}</h2>
        <div class="info">
          <ul>
            <li>км: {{ kmMonth }}</li>
            <li>темп: {{ tempMonth }} мин/км</li>
            <li>тренировок: {{ trainingMonth }}</li>
          </ul>
        </div>
      </div>

      <div class="add-run">
        <BaseButton
          class="add-run-btn"
          form="rectangle"
          text="Добавить пробежку"
          color="yello"
          @click="addNewRun"
        >
        </BaseButton>

        <div class="add-base-information">
          <div class="add-run-input">
            <AddRunBlock text2="км:" placeholder="5" v-model="newRun.distance">
            </AddRunBlock>

            <AddRunBlock
              class="add-run-in1"
              text2="темп:"
              placeholder="5:30"
              v-model="newRun.pace"
            >
            </AddRunBlock>
            <AddRunBlock
              class="add-run-in1"
              text2="время:"
              placeholder="29"
              v-model="newRun.time"
            >
            </AddRunBlock>
          </div>
          <div class="add-run-input-2">
            <Dropwindow
              variation="тип:"
              look="Выберите тип занятия"
              text1="Интервалы"
              text2="Длительная"
              text3="Темповая"
              text4="Восстановительная"
              v-model="newRun.type"
            ></Dropwindow>

            <AddRunBlock
              text2="заметки:"
              placeholder="введите заметку"
              v-model="newRun.notes"
            >
            </AddRunBlock>
          </div>
        </div>
      </div>
    </div>
  </header>
  <main class="main-content">
    <div class="history-run">
      <Title
        text1="История пробежек"
        size="base-text"
        class="history-title"
      ></Title>
      <div class="history-stast">
        <Dropwindow
          variation="Сортировать по типу:"
          look="Все типы"
          text1="Интервалы"
          text2="Длительная"
          text3="Темповая"
          text4="Восстановительная"
          v-model="sortOptions.type"
          sizebig="big"
        ></Dropwindow>

        <Dropwindow
          variation="Cортировать по месяцу:"
          look="Все месяцы"
          text1="Январь"
          text2="Февраль"
          text3="Март"
          text4="Апрель"
          text5="Май"
          text6="Июнь"
          text7="Июль"
          text8="Август"
          text9="Сентябрь"
          text10="Октябрь"
          text11="Ноябрь"
          text12="Декабрь"
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
          <div class="h-container-down-time">{{ run.time }}</div>
          <div class="h-container-down-speed">{{ run.pace }} мин/км</div>
          <div class="h-container-type">{{ run.type || "Тип не выбран" }}</div>
        </div>
        <div class="h-container-bt">
          <BaseButton
            form="very-small"
            text="Удалить"
            color="yello"
            @click="deleteRun(run.id)"
          >
          </BaseButton>
        </div>
      </div>
    </div>
  </main>
</template>

<style>
html,
body {
  margin: 0;
  padding: 0;
  min-height: 100vh; /* Фиксируем высоту */
  width: 100%;
}

body {
  background: linear-gradient(
    180deg,
    rgba(212, 161, 122, 0.8) 0%,
    rgba(196, 78, 24, 0.8) 100%
  );
  background-attachment: fixed; /* Градиент на весь экран */
  position: relative; /* Для абсолютных детей */
}
</style>
<style scoped>
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
  margin-top: 480px; /* Отступ = высота header */
  width: 100%;
  min-height: calc(100vh - 180px); /* Оставшееся пространство */
  box-sizing: border-box;
}
.content-wrapper {
  display: flex;
  justify-content: space-between;
  width: 100%;

  height: auto;
}

.title {
  color: #000000;
  margin: 0;
  font-family: "Times New Roman", Times, serif;
  font-size: 40px;
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
  font-family: "Times New Roman", Times, serif;
}

.add-run {
  position: absolute;
  top: 80%;
  left: 25%;
  width: 625px;
  height: 260px;
  background: linear-gradient(
    180deg,
    rgba(212, 161, 122, 0.8) 0%,
    rgba(196, 78, 24, 0.8) 100%
  );
  font-family: "Times New Roman", Times, serif;
  border-radius: 6px;
  padding: 15px;
}
.history-run {
  width: 1041px;
  min-height: 350px;
  background-color: #f0e68c;
  margin: 0 auto;
  position: relative;
  border-radius: 6px;
  font-family: "Times New Roman", Times, serif;
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
  background: linear-gradient(
    180deg,
    rgba(212, 161, 122, 0.8) 0%,
    rgba(196, 78, 24, 0.8) 100%
  );
  margin-left: 70px;
}
.history-container {
  height: 115px;
  width: 965px;
  background: linear-gradient(
    180deg,
    rgba(212, 161, 122, 0.8) 0%,
    rgba(196, 78, 24, 0.8) 100%
  );
  position: relative;
  margin-left: 35px;
  margin-top: 10px;
  padding: 10px;
  border-radius: 6px;
  font-family: "Times New Roman", Times, serif;
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
  font-family: "Times New Roman", Times, serif;
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
  font-family: "Times New Roman", Times, serif;
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
  font-family: "Times New Roman", Times, serif;
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
  background: linear-gradient(
    180deg,
    rgba(212, 161, 122, 0.8) 0%,
    rgba(196, 78, 24, 0.8) 100%
  );
  display: flex;
  align-items: center;
  border-radius: 6px;
}
</style>
