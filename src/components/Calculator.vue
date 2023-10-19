<template>
  <div class="container box-border h-auto w-80 p-4 border-4 border-black">
    <div class="grid grid-cols-4">
    <div class="col-span-4 box-border h-16 border border-black text-end text-5xl pr-3 mb-2 truncate">{{ totalViews }}</div>

    </div>
    <div class="grid grid-cols-4 gap-2">
      <button @click="clearNumbers" class="rounded text-3xl h-16 w-16 border border-black bg-gray-300 hover:bg-gray-400">C</button>
      <button @click="convertNumberType" class="rounded text-3xl h-16 w-16 border border-black bg-gray-300 hover:bg-gray-400">+/-</button>
      <button @click="percentageNumbers" class="rounded text-3xl h-16 w-16 border border-black bg-gray-300 hover:bg-gray-400">%</button>
      <button @click="operateNumbers('division', 'divide')" :class="[ isActive === 'divide' ? 'bg-green-400' : 'bg-green-300' ]" class="rounded text-3xl h-16 w-16 border border-black bg-green-300 hover:bg-green-400">÷</button>
      <button @click="concatNumbers('7')" class="rounded text-3xl h-16 w-16 border border-black hover:bg-gray-100">7</button>
      <button @click="concatNumbers('8')" class="rounded text-3xl h-16 w-16 border border-black hover:bg-gray-100">8</button>
      <button @click="concatNumbers('9')" class="rounded text-3xl h-16 w-16 border border-black hover:bg-gray-100">9</button>
      <button @click="operateNumbers('multiplication', 'multiply')" :class="[ isActive === 'multiply' ? 'bg-green-400' : 'bg-green-300' ]" class="rounded text-3xl h-16 w-16 border border-black hover:bg-green-400">×</button>
      <button @click="concatNumbers('4')" class="rounded text-3xl h-16 w-16 border border-black hover:bg-gray-100">4</button>
      <button @click="concatNumbers('5')" class="rounded text-3xl h-16 w-16 border border-black hover:bg-gray-100">5</button>
      <button @click="concatNumbers('6')" class="rounded text-3xl h-16 w-16 border border-black hover:bg-gray-100">6</button>
      <button @click="operateNumbers('subtraction', 'subtract')" :class="[ isActive === 'subtract' ? 'bg-green-400' : 'bg-green-300' ]" class="rounded text-3xl h-16 w-16 border border-black hover:bg-green-400">-</button>
      <button @click="concatNumbers('1')" class="rounded text-3xl h-16 w-16 border border-black hover:bg-gray-100">1</button>
      <button @click="concatNumbers('2')" class="rounded text-3xl h-16 w-16 border border-black hover:bg-gray-100">2</button>
      <button @click="concatNumbers('3')" class="rounded text-3xl h-16 w-16 border border-black hover:bg-gray-100">3</button>
      <button @click="operateNumbers('addition', 'add')" :class="[ isActive === 'add' ? 'bg-green-400' : 'bg-green-300' ]" class="rounded text-3xl h-16 w-16 border border-black hover:bg-green-400">+</button>
      <button @click="concatNumbers('0')" class="rounded text-3xl h-16 w-33 border border-black hover:bg-gray-100 col-span-2">0</button>
      <button @click="concatNumbers('.')" class="rounded text-3xl h-16 w-16 border border-black hover:bg-gray-100">·</button>
      <button @click="getTotalNumbers" class="rounded text-3xl h-16 w-16 border border-black bg-green-300 hover:bg-green-400">=</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Calculator',
  data () {
    return {
      total: 0,
      firstNum: 0,
      secondNum: 0,
      isActive: '',
      currentMethod: '',
      operators: {
        addition: '+',
        subtraction: '-',
        multiplication: '*',
        division: '/'
      },
      equalCount: 0
    }
  },
  computed: {
    totalViews () {
      return parseFloat(this.total).toLocaleString('en-US')
    }
  },
  methods: {
    concatNumbers(num) {
      // console.log(this.operators[this.currentMethod])
      this.equalCount = 0
      if (this.isActive) {
        this.isActive = ''
        this.total = 0
      }

      if (this.total.length !== 7) {
        if (this.total === 0) {
          this.total = ''
        }
        this.total = this.total.toString()
        if (num === '.' && this.total.includes('.')) {
          return false
        }
        this.total = this.total + num
      }
    },
    clearNumbers() {
      this.total = 0
      this.firstNum = 0
      this.secondNum = 0
      this.currentMethod = ''
      this.isActive = ''
      this.equalCount = 0
    },
    getTotalNumbers() {
      this.isActive = ''
      if (this.equalCount === 0) {
        this.secondNum = this.total
        this.equalCount++
      }
      if (this.currentMethod === 'addition') {
        this.firstNum = parseFloat(this.firstNum) + parseFloat(this.secondNum)
        this.total = this.firstNum
      }

      if (this.currentMethod === 'subtraction') {
        this.firstNum = parseFloat(this.firstNum) - parseFloat(this.secondNum)
        this.total = this.firstNum
      }

      if (this.currentMethod === 'multiplication') {
        this.firstNum = parseFloat(this.firstNum) * parseFloat(this.secondNum)
        this.total = this.firstNum
      }

      if (this.currentMethod === 'division') {
        this.firstNum = parseFloat(this.firstNum) / parseFloat(this.secondNum)
        this.total = this.firstNum
      }
    },
    convertNumberType() {
      this.equalCount = 0
      this.total = this.total * -1
    },
    percentageNumbers () {
      this.total = this.total / 100
    },
    operateNumbers(method, isActive) {
      this.isActive = isActive

      if (this.equalCount !== 0) {
        this.equalCount = 0
        this.currentMethod = ''
      }

      if (this.currentMethod === method) {
        this.firstNum = eval(`${this.firstNum} ${this.operators[this.currentMethod]} ${this.total}`)
        this.total = this.firstNum
      }

      if (this.currentMethod !== method && this.currentMethod !== '') {
        this.firstNum = eval(`${this.firstNum} ${this.operators[this.currentMethod]} ${this.total}`)
        this.total = this.firstNum
        this.currentMethod = method
      }

      if (this.currentMethod === ''){
        this.currentMethod = method
        this.firstNum = parseFloat(this.total)
      }
    }
  },
}
</script>