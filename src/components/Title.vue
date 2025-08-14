<script setup>
const props = defineProps({
  color: {
    type: String,
    default: 'black'
  },
  size: {
    type: String,
    default: 'small'
  },
  text: {
    type: String,
    default: 'Текст'
  }
})
</script>

<template>
  <div>
    <p class="base-text" :class="[color, size]">
      {{ text }}
    </p>
  </div>
</template>

<style scoped>
.base-text {
  font-family: "Times New Roman", Times, serif;
  line-height: 28px;
  color: black;
}

.small {
  font-size: 24px;
}
.big {
  font-size: 40px;
}

.grey {
  color: #696969;
}
</style>
