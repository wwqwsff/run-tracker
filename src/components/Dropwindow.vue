<script setup>
const props = defineProps({
  modelValue: {
    type: String,
    default: "",
  },
  variation: {
    type: String,
    default: "Тип",
  },
  look: {
    type: String,
    default: "Выберите",
  },
  text1: String,
  text2: String,
  text3: String,
  text4: String,
  text5: String,
  text6: String,
  text7: String,
  text8: String,
  text9: String,
  text10: String,
  text11: String,
  text12: String,
  sizebig: {
    type: String,
    default: "add-run-select",
  },
  writebig: {
    type: String,
    default: "add-run-input-type",
  },
});
const emit = defineEmits(["update:modelValue"]);

const handleChange = (e) => {
  emit("update:modelValue", e.target.value);
};
</script>
<template>
  <div class="add-run-sel">
    <div class="add-run-input-type" :class="[writebig]">{{ variation }}</div>
    <select
      class="add-run-select"
      :class="[sizebig]"
      :value="modelValue"
      @change="handleChange"
    >
      <option value="">{{ look }}</option>
      <option :value="text1">{{ text1 }}</option>
      <option :value="text2">{{ text2 }}</option>
      <option :value="text3">{{ text3 }}</option>
      <option :value="text4">{{ text4 }}</option>
      <option :value="text5">{{ text5 }}</option>
      <option :value="text6">{{ text6 }}</option>
      <option :value="text7">{{ text7 }}</option>
      <option :value="text8">{{ text8 }}</option>
      <option :value="text9">{{ text9 }}</option>
      <option :value="text10">{{ text10 }}</option>
      <option :value="text11">{{ text11 }}</option>
      <option :value="text12">{{ text12 }}</option>
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
  background-color: #f0e68c;
  border-radius: 6px;
  margin-left: 30px; /* Добавляем отступ слева */
  border: 1px solid black;
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
.bigtext {
  font-size: 20px;
  margin-left: 70px;
}
</style>
