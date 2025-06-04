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

</style>
