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
    text1: {
        type: String,
        default: 'Текст'
    },}
)

</script>

<template>
    <div>
        <p  class="base-text" 
            :class="[color, size]">
            {{text1}}
        </p>
    </div>
</template>


<style scoped>
.base-text{
    font-size: 24px;
    color:black
}
.big{
    font-size:36px;
    color:black
}
.grey{
    color:#696969 ;
}
</style>