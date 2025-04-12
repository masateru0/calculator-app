<template>
    <button @click="handleClick" class="button">
        {{ label }}
    </button>
</template>

<script setup>
defineProps({
    label: {
        type: String,
        required: true
    }
});

const hadleClick = () => {
    emit('click', label)
}
</script>

<style scoped>
.button {
    background: #fff;
    border: none;
    
}
</style>