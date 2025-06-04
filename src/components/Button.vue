<template>
    <button @click="handleClick" :class="['button', getButtonClass()]">
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

const getButtonClass = () => {
    if (props.label === 'AC' || props.label === 'C') return 'control-button'
    if (props.label === '=') return 'operator-button'
    return ''
}
</script>

<style scoped>
.button {
    padding: 15px;
    font-size: 20px;
    border-radius: 5px;
    text-align: center;
    border: none;
    background-color: #dcdcdc;
    cursor: pointer;
}
.button:hover {
    background-color: #d3d3d3;
}

.control-button {
    background-color: #FA8072;
}

.control-button:hover {
    background-color: #FF4F50;
}

.operator-button {
    background-color: #90EE90;
}

.operator-button:hover {
    background-color: #32CD32;
}
</style>