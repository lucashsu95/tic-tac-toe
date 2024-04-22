<script setup>
import { ref } from 'vue'
const isWin = ref(null)

const robotMod = ref(false)
const clickOrder = ref([])
let flag = true

const userClick = (e) => {
  if (isWin.value || e.target.textContent) return

  if (robotMod.value) {
    e.target.textContent = 'X'
    setTimeout(() => {
      robotClick()
    }, 200)
  } else {
    e.target.textContent = flag ? 'X' : 'O'
    flag = !flag
  }
  checkOrder(e.target.dataset.key)
  checkWin()
}

const robotClick = () => {
  if (isWin.value) return
  const divs = [...document.querySelectorAll('.box > div')]
  const can = divs.filter((x) => x.textContent === '')
  const rand = Math.floor(Math.random() * can.length)

  can[rand].textContent = 'O'
  checkOrder(can[rand].dataset.key)
  checkWin()
}

const checkOrder = (key) => {
  clickOrder.value.push(key)

  if (clickOrder.value.length > 6) {
    const remove = clickOrder.value.shift()
    document.querySelector(`[data-key='${remove}']`).textContent = ''
  }
}

const wins = ['123', '456', '789', '147', '258', '369', '159', '357']
const checkWin = () => {
  const divs = [...document.querySelectorAll('.box > div')]
  const o = divs
    .filter((x) => x.textContent === 'O')
    .map((x) => x.dataset.key)
    .join('')
  const x = divs
    .filter((x) => x.textContent === 'X')
    .map((x) => x.dataset.key)
    .join('')
  for (let win of wins) {
    const [a, b, c] = win.split('')
    const check = (n) => n.includes(a) && n.includes(b) && n.includes(c)
    isWin.value = check(o) ? 'O' : check(x) ? 'X' : null
    if (isWin.value) break
  }
}

const resetGame = () => {
  isWin.value = null
  ;[...document.querySelectorAll('.box > div')].map((x) => (x.textContent = ''))
}
</script>

<template>
  <h1>圈圈差差</h1>
  {{ clickOrder }}
  <label for="robotMod">邊緣人模式</label><input type="checkbox" v-model="robotMod" id="robotMod" />
  <h2 v-show="isWin">{{ isWin }} Win!</h2>
  <div class="box">
    <div v-for="i in 9" :key="i" @click="userClick" :data-key="i"></div>
  </div>
  <button class="btn btn-primary mt-3" @click="resetGame">Reset</button>
</template>

<style>
.box {
  display: grid;
  width: 400px;
  height: 400px;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: repeat(3, 1fr);

  & > div {
    font-size: 48px;
    border: 1px solid #ccc;
    margin: 5px;
    border-radius: 0.375rem;
    display: flex;
    justify-content: center;
    align-items: center;
    transition: 0.5s;
  }
}
</style>
