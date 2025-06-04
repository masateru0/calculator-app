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
    // Error状態中は "C" または "AC" 以外は無視する
    if (display.value === 'Error' && btn !== 'C' && btn !== 'AC') {
        return
    }

    switch (btn) {
        case 'AC':
            display.value = '0'
            isResultShown.value = false // 結果表示状態もリセット
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

            // 数字が17桁を超えたらエラー表示（演算子・記号は除外）
            const numericLength = display.value.replace(/[^0-9.]/g, '').length
            if (isNumberInput && numericLength >= 17) {
                display.value = 'Error'
                isResultShown.value = true
                break
            }

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
    background-color: #2a0a89;
    width: 300px;
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0px 4px 10px 0 rgba(0, 0, 0, 0.2);
}

.display {
    width: 100%;
    height: 50px;
    border-radius: 5px;
    border: 1px solid #ddd;
    background-color: #ccc;
    padding: 5px 10px;
    text-align: right;
    font-size: 35px;
    margin-bottom: 20px;
    box-sizing: border-box;
}

.button-panel {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 10px;
}
</style>

<style>
body {
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: #F3FFD8;
    margin: 0;
    height: 100vh;
    align-items: center;
}
</style>