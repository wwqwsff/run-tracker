<script setup>
const props = defineProps({
    text2:{
        type: String,
        default:'км:'
    },
    text3: {
        type: String,
        default:'Введите данные'
    },
    writebig:{
        type: String,
        default:'label'
    },
    bigcont:{
        type: String,
        default:'input-style"'
    }
})
</script>

<template>
     <div class="base-form">
       <div class="label" :class="[writebig]">{{ text2 }}</div> 

        <div class="bace-form-input">
            <input type="text" class="input-style" :placeholder="text3" :class="[bigcont]">
        </div>
     </div>
</template>

<style scoped>
.input-style{
    height: 30px;
    width: 130px;
    padding: 5px;
    box-sizing: border-box;
    background-color:#F0E68C;
    border-radius: 6px;
    border: 1px solid black; 
}


.input-style::placeholder {
    color: black;
    opacity: 1; 
}

.base-form{
    display: flex;
    align-items: center; 
    gap: 60px; 
    margin-bottom: 27px;
    
}
.label {
    width: 50px; 
    text-align: left;
    position: relative;
    top: 2px; 
    left: 30px;
    color: black;
}
.bigtext{
    font-size: 20px;
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
</style>