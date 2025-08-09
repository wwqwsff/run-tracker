<script setup>
const props = defineProps({
    quality: {
        type: String,
        required: true
    },
    value: {
        type: Number,
        required: true
    },
})
</script>

<template>
    <div class="run-data-item">
        <div class="run-data-item-quality">{{ quality }}</div>
        <div class="run-data-item-value">{{ value }}</div>
    </div>
</template>

<style scoped>
.run-data-item {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    width: 185px;
    height: 65px;
    background: linear-gradient(
        180deg,
        rgba(212, 161, 122, 0.8) 0%,
        rgba(196, 78, 24, 0.8) 100%
    );
    border-radius: 6px;
}

.run-data-item-quality {
    font-size: 16px;
    font-weight: 400;
    line-height: 24px;
    color: #000000;
}

.run-data-item-value {
    font-size: 24px;
    font-weight: 400;
    line-height: 32px;
    color: #000000;
}
</style>