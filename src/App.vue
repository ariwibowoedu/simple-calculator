<template>
  <div class="min-h-screen bg-gray-100 flex items-center justify-center">
    <div class="bg-white p-6 rounded-xl shadow-md w-full max-w-sm">
      <!-- INPUT -->
      <input
        v-model="display"
        class="w-full text-right text-2xl font-mono mb-4 border border-gray-300 rounded px-2 py-1 focus:outline-none focus:ring-2 focus:ring-blue-400"
        @keydown.enter.prevent="press('=')"
        @input="filterInput"
        placeholder="0"
        ref="inputRef"
      />

      <!-- BUTTONS -->
      <div class="grid grid-cols-4 gap-2">
        <button
          v-for="btn in buttons"
          :key="btn"
          @click="press(btn)"
          class="p-4 bg-gray-200 rounded hover:bg-gray-300 font-semibold"
          :class="btn === 'C' ? 'col-span-4 bg-red-300 hover:bg-red-400 text-white' : ''"
        >
          {{ btn }}
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'

const display = ref('')
const inputRef = ref(null)

const buttons = [
  'C',
  '7',
  '8',
  '9',
  '/',
  '4',
  '5',
  '6',
  '*',
  '1',
  '2',
  '3',
  '-',
  '0',
  '.',
  '=',
  '+',
]

const filterInput = () => {
  display.value = display.value.replace(/[^0-9+\-*/.]/g, '')
}

const press = (btn) => {
  if (btn === 'C') {
    display.value = ''
  } else if (btn === '=') {
    try {
      display.value = calculate(display.value)
    } catch {
      display.value = 'Error'
    }
  } else {
    display.value += btn
  }
}

// Fungsi kalkulasi manual
function calculate(expr) {
  const tokens = expr.match(/(\d+(\.\d+)?|[+\-*/])/g)
  if (!tokens) return 'Error'

  let result = parseFloat(tokens[0])
  for (let i = 1; i < tokens.length; i += 2) {
    const operator = tokens[i]
    const nextNumber = parseFloat(tokens[i + 1])
    if (isNaN(nextNumber)) return 'Error'

    switch (operator) {
      case '+':
        result += nextNumber
        break
      case '-':
        result -= nextNumber
        break
      case '*':
        result *= nextNumber
        break
      case '/':
        if (nextNumber === 0) return 'Error'
        result /= nextNumber
        break
      default:
        return 'Error'
    }
  }

  return result.toString()
}

onMounted(() => {
  inputRef.value?.focus()
})
</script>
