<script setup>
const props = defineProps({
  modelValue: {
    type: String,

    default: ''
  },
  variation: {
    type: String,
    default: 'Тип'
  },
  look: {
    type: String,
    default: 'Выберите'
  },
  text: {
    type: String,
    required: true
  },
  sizebig: {
    type: String,
    default: 'add-run-select'
  },
  writebig: {
    type: String,
    default: 'add-run-input-type'
  },
  options: {
    type: Array,
    default: () => []
  }
})
const emit = defineEmits(['update:modelValue'])

const handleChange = e => {
  emit('update:modelValue', e.target.value)
}
</script>
<template>
  <div class="add-run-sel">
    <div class="add-run-input-type" :class="[writebig]">{{ variation }}</div>
    <select class="add-run-select" :class="[sizebig]" :value="modelValue" @change="handleChange">
      <option value="">{{ look }}</option>
      <option v-for="option in options" :key="option" :value="option">{{ option }}</option>
    </select>
  </div>
</template>

<style scoped>
.add-run-sel {
  display: flex;
  align-items: center;
  gap: 20px;
  margin-bottom: 27px;
}
.add-run-input-type {
  width: auto;
  text-align: left;
  color: black;
  margin-left: 30px;
}
.add-run-select {
  height: 30px;
  width: 130px;
  padding: 5px;
  box-sizing: border-box;
  background-color: #f0e68c;
  border-radius: 6px;
  margin-left: 30px;
  border: 1px solid black;
}
.big {
  height: 30px;
  width: 170px;
  padding: 5px;
  box-sizing: border-box;
  background-color: #f0e68c;
  border-radius: 6px;
  margin-left: 30px;
  border: 1px solid black;
}
.bigtext {
  font-size: 20px;
  margin-left: 70px;
}
</style>
