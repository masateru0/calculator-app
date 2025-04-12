<template>
    <div class="display">{{ value }}</div>
</template>

<script setup>
defineProps({
    value: {
        type:String,
        required: true
    }
})
</script>

<style scoped>
.display {
    background: #eee;
    font-size: 24px;
    text-align: right;
    padding: 10px;
    border-radius: 4px;
    margin-bottom: 10px;
}
</style>