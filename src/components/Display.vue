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
    white-space: nowrap;       /* テキストの折り返しを防ぐ */
    overflow: hidden;          /* 超えたら表示しない */
    text-overflow: ellipsis;   /* 必要なら…で省略 */
    max-width: 100%;           /* 親の幅に収める */
    box-sizing: border-box;    /* padding含めて幅を計算 */
}
</style>