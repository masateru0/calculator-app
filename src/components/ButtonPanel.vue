<template>
    <div class="button-panel">
        <Button
            v-for="button in buttons"
            :key="button"
            :label="button"
            @click="handleClick"
        />
    </div>
</template>

<script setup>
import Button from '@/components/Button.vue'

defineProps({
    buttons: {
        type: Array,
        required: true
    }
})

const emit = defineEmits(['buttonClick'])

const handleClick = (button) => {
    emit('buttonClick', button)
}
</script>

<style scoped>
.button-panel {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 10px;
}
</style>