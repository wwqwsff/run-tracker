<script setup>
const props = defineProps({
  text2: {
    type: String,
    default: 'км:'
  },
  text3: {
    type: String,
    default: 'Введите данные'
  },
  hasError: {
    type: Boolean,
    default: false
  },

  writebig: {
    type: String,
    default: 'label'
  },
  bigcont: {
    type: String,
    default: 'input-style"'
  },
  modelValue: [String, Number],
  placeholder: String
})
const emit = defineEmits(['update:modelValue'])

const handleInput = e => {
  if (props.text2 === 'темп:') {
    const value = e.target.value.replace(/[^\d:]/g, '')
    emit('update:modelValue', value)
  } else {
    const value = e.target.value.replace(/[^\d.]/g, '')
    emit('update:modelValue', value)
  }
}
</script>

<template>
  <div class="base-form">
    <div class="label" :class="[writebig]">{{ text2 }}</div>

    <div class="bace-form-input">
      <input
        type="text"
        class="input-style"
        :class="[bigcont, { 'error-input': hasError }]"
        :value="modelValue"
        @input="handleInput"
        :placeholder="placeholder"
      />>
    </div>
  </div>
</template>

<style scoped>
.input-style {
  height: 30px;
  width: 130px;
  padding: 5px;
  box-sizing: border-box;
  background-color: #f0e68c;
  border-radius: 6px;
  border: 1px solid black;
}

.input-style::placeholder {
  color: black;
  opacity: 1;
}

.base-form {
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
.bigtext {
  font-size: 20px;
}
.big {
  height: 30px;
  width: 170px;
  padding: 5px;
  box-sizing: border-box;
  background-color: #f0e68c;
  border-radius: 6px;
  margin-left: 30px; /* Добавляем отступ слева */
  border: 1px solid black;
}
.error-input {
  border: 1px solid red !important;
  box-shadow: 0 0 5px rgba(255, 0, 0, 0.5);
}

/* Усиленная подсветка для ошибки */
.error-style {
  border: 2px solid red !important;
  background-color: #ffe6e6 !important;
}
</style>
