<script setup>
const props = defineProps({
    form: {
        type: String,
        default: 'rectangle'
    },
    text: {
        type: String,
        default: 'Добавить пробежку'
    },
    color: {
        type: String,
        default: 'blue'
    }
})
defineEmits(['click'])
</script>

<template>
  <button class="base-button" 
          :class="[form, color]"  
          @click="$emit('click')">
    {{ text }}  <!-- Отображаем текст из props -->
  </button>
</template>


<style>
.base-button{
  border-radius: 6px;
  font-family: "Times New Roman", Times, serif; 
  border: 1px solid black; 
}
.rectangle{
    width: 380px;
    height: 36px;
}
.yello{
    background-color: #F0E68C;
}
.rectangle-small{
    width: 130px;
    height: 33px;
    border-radius: 6px;
}
.very-small{
    height:20px ;
    width:130px ;
    border-radius: 6px;
    border: none;
    font-family: "Times New Roman", Times, serif; 
    border: 1px solid black; 
    font-size:15px;
    color:black;
    padding: 0;
}
</style>
