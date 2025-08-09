<script setup>
import { computed, ref } from "vue";
import { AddRunWindow, BaseButton, Dropwindow, InputWithText, RunDataItem, Title } from "./components";
import { monthName, monthNameNumber, dayHumber, dayName } from "./mock-api";


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

const infoItems = computed(() => {
  return [
    { quality: "км", value: kmMonth.value },
    { quality: "темп", value: tempMonth.value },
    { quality: "тренировок", value: trainingMonth.value },
  ];
});

const addNewRun = (run) => {
  runs.value.unshift(run);
};
</script>

<template>
  <header class="header">
    <Title text="Беговой трекер" size="big"  class="title"/>
   

    <div class="content-wrapper">
      
     
          <div class="info-items">
            <h2 class="montName">{{ nowmonth.month }}</h2>
            <RunDataItem v-for="item in infoItems" :key="item.quality" :quality="item.quality" :value="item.value" />
          </div>

          <AddRunWindow @addNewRun="addNewRun" />
    
    </div>
  </header>
  
<!-- вынести в отдельный компонент -->

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
  margin-top: 480px; /* Отступ = высота header */
  width: 100%;
  min-height: calc(100vh - 180px); /* Оставшееся пространство */
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
  font-family: "Times New Roman", Times, serif;
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


.montName {
  font-size: 24px;
  text-align: left;
  margin: 0 0 20px 0;
  font-family: "Times New Roman", Times, serif;
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
