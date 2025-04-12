<template>
    <button @click="handleClick" class="button">
        {{ label }}
    </button>
</template>

<script setup>
const props = defineProps({
    label: {
        type: String,
        required: true
    }
});

const emit = defineEmits(['click'])

const handleClick = () => {
    emit('click', props.label) // emit を使用して親コンポーネントにイベントを送信
}
</script>

<style scoped>
.button {
    padding: 15px;
    font-size: 18px;
    border: none;
    border-radius: 6px;
    background-color: #ddd;
    cursor: pointer;
}
.button:hover {
    background-color: #ccc;
}
</style>