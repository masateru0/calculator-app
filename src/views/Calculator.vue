<template>
    <div class="calculator">
        <Display :value="display" />
        <ButtonPanel :buttons="buttons" @buttonClick="handleClick" />
    </div>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue';
import Display from '@/components/Display.vue'
import ButtonPanel from '@/components/ButtonPanel.vue';

const display = ref('0')
const isResultShown = ref(false) // 新しく追加した状態管理

const buttons = [
    'AC', 'C', '%', '÷',
    '7', '8', '9', '×',
    '4', '5', '6', '-',
    '1', '2', '3', '+',
    '00', '0', '.', '=',
]

const handleClick = (btn) => {
    switch (btn) {
        case 'AC':
            display.value = '0'
            break

        case 'C':
            display.value = display.value.slice(0, -1) || '0'
            break

        case '=':
            try {
                const result = eval(
                    display.value
                        .replace(/×/g, '*')
                        .replace(/÷/g, '/')
                        .replace(/-/g, '-')
                        .replace(/\+/g, '+')
                )

                // 少数誤差を防ぐ処理：少数第10位までで四捨五入
                const rounded = Math.round((result + Number.EPSILON) * 1e10) / 1e10 // 小数点以下10桁まで表示

                display.value = String(rounded)
                isResultShown.value = true // 結果が表示されたことを示す
            } catch {
                display.value = 'Error'
            }
            break

        default:
            // 数字 or "." を押した時、計算直後なら上書き
            const isNumberInput = /^[0-9.]+$/.test(btn)

            if (isResultShown.value && isNumberInput) {
                display.value = btn
            } else if (display.value === '0' && !['.', '00'].includes(btn)) {
                display.value = btn
            } else {
                display.value += btn
            }
            // 数字が押されたら、結果表示状態を解除
            isResultShown.value = false
            break
    }
}

// キーボード対応処理
const handleKeydown = (e) => {
    const key = e.key

    const keyMap = {
        '*': '×',
        '/': '÷',
        '+': '+',
        '-': '-',
        Enter: '=',
        '=': '=',
        Backspace: 'C',
        Escape: 'AC',
        '%': '%',
        '.': '.',
    }

    if (key >= '0' && key <= '9') {
        handleClick(key)
    } else if (key in keyMap) {
        handleClick(keyMap[key])
    }
}

// コンポーネントがマウントされたときにキーボードイベントをリッスン
onMounted(() => {
    window.addEventListener('keydown', handleKeydown)
})

// コンポーネントがアンマウントされるときにイベントリスナーを削除
onBeforeUnmount(() => {
    window.removeEventListener('keydown', handleKeydown)
})
</script>

<style scoped>
.calculator {
    max-width: 300px;
    margin: auto;
    margin-top: 50px;
    border: 2px solid #ccc;
    padding: 10px;
    border-radius: 8px;
    background: #f9f9f9;
}
</style>