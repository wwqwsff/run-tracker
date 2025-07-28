<script setup>
import { computed,ref } from 'vue';
import BaseButton from './BaseButton.vue';
import Title from './Title.vue'
import AddRunBlock from './AddRunBlock.vue';
import Dropwindow from './Dropwindow.vue';
const monthName = [ 'Январь', 'Февраль', 'Март', 'Апрель', 
  'Май', 'Июнь', 'Июль', 'Август',
  'Сентябрь', 'Октябрь', 'Ноябрь', 'Декабрь' ]
const monthNameNumber =[ '01', '02','03','04','05','06','07','09','10','11','12']
const dayHumber = [ '01', '02','03','04','05','06','07','08','09','10','11','12',
'13','14','15','16','17','18','19','20','21','22','23','24','25','26','27','28',
'29','30','31']
const dayName = ['Вс', 'Пн', 'Вт', 'Ср', 'Чт', 'Пт', 'Сб']
const nowmonth = computed(()=>{
  const now = new Date()
  return {
    month: monthName[now.getMonth()],
    monthNumber: monthNameNumber[now.getMonth()],
    dayNumber: dayHumber[now.getDate()-1],
    dayName: dayName[now.getDay()]
  }
})

  
const kmMonth = computed(()=>{
  return 1
})
const tempMonth = computed(()=>{
  return '5:30'
})
const trainingMonth = computed(()=>{
  return 5
}) 
const kmrunning = computed(()=>{
  return 5.1
})
</script>

<template>
  <header class="header">
    <div class="title">
      <Title text1 ='Беговой трекер' size = 'big'></Title>
    </div>

    <div class="content-wrapper">
      <div class="month">
        <h2 class="montName">{{ nowmonth.month }}</h2>
        <div class="info">
          <ul>
            <li>км: {{ kmMonth}}</li>
            <li> темп: {{ tempMonth }} мин/км </li>
            <li> тренировок: {{ trainingMonth }}</li>
          </ul>
        </div>
      </div>
      
      <div class="add-run">
        
              <BaseButton  
            class="add-run-btn" 
            form="rectangle" 
            text="Добавить пробежку"
            color="yello">
            </BaseButton>
            
            <div class="add-base-information">
                <div class="add-run-input">
                  <AddRunBlock 

                  text2 = 'км:' 
                  text3 = '5'>
                  </AddRunBlock>

                  <AddRunBlock 
                  class="add-run-in1"
                  text2 = 'темп:' 
                  text3 = '5:30'>
                  </AddRunBlock>
                  <AddRunBlock 
                  class="add-run-in1"
                  text2 = 'время:' 
                  text3 = '29'>
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
              
              ></Dropwindow>
            
              <AddRunBlock
              text2 = 'заметки:'
              text3 ='введите заметку'>
              </AddRunBlock>
            </div>
             
          </div>
    </div>
    
    
  </div>
 

  </header>
  <main class="main-content">
   <div class="history-run">
    <Title text1 ='История пробежек' size = 'base-text' class="history-title"></Title>
      <div class="history-stast">
        <Dropwindow 
          variation="Сортировать по:"
          look="Выберите тип занятия"
          text1="Интервалы"
          text2="Длительная"
          text3="Темповая"
          text4="Восстановительная"
          
          sizebig="big"
        ></Dropwindow>
        <AddRunBlock
          text2="Месяцy:"
          text3="Введите месяц"
          bigcont="big"
          >
        </AddRunBlock>
        <BaseButton
        form="rectangle-small"
        text="Применить"
        class="btn-history"></BaseButton>
      </div>
      <div class="history-container">
         <div class="h-container-up">
            <div class="h-container-up-date">{{nowmonth.dayNumber}}.{{ nowmonth.monthNumber }} {{ nowmonth.dayName }}</div>
            <input type="text" class="h-container-up-notes" placeholder="Мои заметки">
          </div>
         <div class="h-container-distance">{{kmrunning}} км</div>
         <div class="h-container-down">
            <div class="h-container-down-time">49</div>
            <div class="h-container-down-speed">5:14</div>
            <div class="h-container-type">темповая</div>
          </div>
            <div class="h-container-bt">
                <BaseButton 
                form="very-small"
                text="Удалить"
                color="yello">
              </BaseButton>
                <BaseButton  
                form="very-small"
                text="Редактировать"
                color="yello"
              ></BaseButton>
            </div>
         
      </div>
    </div>
    
      
  </main>
</template>

<style>

html, body {
  margin: 0;
  padding: 0;
  min-height: 100vh; /* Фиксируем высоту */
  width: 100%;
}

body {
  background: linear-gradient(180deg, 
              rgba(212, 161, 122, 0.8) 0%, 
              rgba(196, 78, 24, 0.8) 100%);
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
 ;
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
.content-wrapper{
  display: flex;
  justify-content: space-between;
  width: 100%;
  
  height: auto

  
}

.title {
  color: #000000;
  margin: 0;
  font-family: 'Times New Roman', Times, serif;
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
  font-family: 'Times New Roman', Times, serif;
}

.add-run{
  position: absolute;
  top: 80%;
  left: 25%;
  width: 625px;
  height: 260px;
  background: linear-gradient(180deg, rgba(212, 161, 122, 0.8) 0%, rgba(196, 78, 24, 0.8)100%);
  font-family: 'Times New Roman', Times, serif;
  border-radius: 6px;
  padding: 15px;
  
  
}
.history-run{
  width: 1041px;
  min-height:350px;
  background-color: #F0E68C;
  margin: 0 auto;
  position: relative;
  border-radius: 6px;
  font-family: 'Times New Roman', Times, serif;
}
.history-title{
  padding-top: 10px;
   font-weight: bold;
}
.history-stast{
  display: flex;
  flex-direction:row ;
  gap: 30px; /* Заменит margin-right у .add-run-input */
  padding: 20px 0 0 ;
  margin-top:25px;
}
.btn-history{
    background: linear-gradient(180deg, rgba(212, 161, 122, 0.8) 0%, rgba(196, 78, 24, 0.8)100%);
    margin-left: 70px;
}
.history-container{
  height: 115px;
  width: 965px;
  background: linear-gradient(180deg, rgba(212, 161, 122, 0.8) 0%, rgba(196, 78, 24, 0.8)100%);
  position: relative;
  margin-left: 35px;
  margin-top: 10px;
  padding: 10px;
  border-radius: 6px;
  font-family: 'Times New Roman', Times, serif;
}
.h-container-up{
  display: flex;
  flex-direction:row ;
  gap: 700px
}
.h-container-up-date{
  
  height: 17px;
  width: 100px;
  font-size: 15px;
  text-align: center;
  margin: 0 0 20px 0; 
  font-family: 'Times New Roman', Times, serif;
  color: #000000;
  
}
.h-container-up-notes{
  height: 45px;
  width: 285px;
  background-color: #F0E68C;
  border-radius: 6px;
  border: 1px solid black;  
  padding-left: 10px;
  color:#000000;
}
.h-container-distance{
  height: 35px;
  width:130px ;
  text-align: center;
  color:#000000;
  margin-left: 200px;
  font-size: 20px;
  font-weight: bold;
}
.h-container-down{
  display: flex;
  flex-direction:row ;
  gap: 235px; /* Заменит margin-right у .add-run-input */
  margin-top:15px;
  color:#3b3a3a;
}
.h-container-bt{
  display: flex;
  flex-direction:row ;
  gap: 35px; 
  margin-left:700px;
  margin-top:-20px;
}

.add-base-information{
  display: flex;
  flex-direction:row ;
  gap: 5px; /* Заменит margin-right у .add-run-input */
  padding: 20px 0 0 ;
  margin-top:25px;
}
.add-run-border{
   width: 625px;
    height: 330px;
    border: 2px solid black;
    margin: 0 auto;
    padding: 17px 0 0 28px;
    text-align: left;
}

.add-run-input{
  margin-top: 3px;
  margin-right: 20px;
  padding: 0px 20px 0 0;
  
}
.add-run-in1{
  margin-top: 27px;
}

.montName {
  font-size: 24px;
  text-align: left;
  margin: 0 0 20px 0; 
  font-family: 'Times New Roman', Times, serif;
  color: #000000;
}
.add-run-btn{
  margin: 0 0 0px 0 ;
}
.info {
  width: 100%;
}

.add-run-input-2{
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
  color:black;
  text-align: left;
  font-family: 'Times New Roman', Times, serif;
  
}



@media (max-width: 768px) {
  .title {
    font-size: 32px;
  }
  
  .montName, ul {
    font-size: 20px;
  }
}

@media (max-width: 480px) {
  .title {
    font-size: 28px;
  }
  
  .montName, ul {
    font-size: 18px;
  }
  
  header {
    padding: 1px 15px;
  }
}
li{
  margin-bottom:20px;
  width: 185px;
  height: 65px;
  background: linear-gradient(180deg, rgba(212, 161, 122, 0.8) 0%, rgba(196, 78, 24, 0.8)100%);
  display: flex;
  align-items: center;
  border-radius: 6px; 
}

</style>