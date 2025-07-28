<script setup>
const props = defineProps({
    
    variation: {
        type: String,
        default: 'Тип'
    },
    look: {
        type: String,
        default: '1'
    },
     text1: {
        type: String,
        default: '1'
    },
     text3: {
        type: String,
        default: '1'
    },
     text4: {
        type: String,
        default: '1'
    },
     text2: {
        type: String,
        default: '1'
    },
       sizebig: {
        type: String,
        default: 'add-run-select'
    },
    writebig: {
        type: String,
        default: 'add-run-input-type'
    }
})
defineEmits(['click'])
</script>
<template>
<div class="add-run-sel">
    <div class="add-run-input-type" :class="[writebig]">{{ variation }} </div>  
    <select class="add-run-select" :class="[sizebig]">
        <option value="">{{ look }}</option>
        <option value="1">{{ text1 }}</option>
        <option value="2">{{ text2 }}</option>
        <option value="3">{{ text3 }}</option>
        <option value="4">{{ text4 }}</option>
    </select>
</div>
    </template>

<style scoped>
.add-run-sel{
   display: flex;
    align-items: center; 
    gap: 20px; 
    margin-bottom: 27px;
}
.add-run-input-type {
  width: auto; /* Убираем фиксированную ширину */
  text-align: left;
  color: black;
  margin-left: 30px; /* Заменяем left на margin */
  
}
.add-run-select {
  height: 30px;
  width: 130px;
  padding: 5px;
  box-sizing: border-box;
  background-color: #F0E68C;
  border-radius: 6px;
  margin-left: 30px; /* Добавляем отступ слева */
  border: 1px solid black; 
}
.big{
  height: 30px;
  width: 170px;
  padding: 5px;
  box-sizing: border-box;
  background-color: #F0E68C;
  border-radius: 6px;
  margin-left: 30px; /* Добавляем отступ слева */
  border: 1px solid black;   
}
.bigtext{
    font-size: 20px;
    margin-left: 70px;
}
</style>