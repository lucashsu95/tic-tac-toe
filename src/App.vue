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
    setTimeout(robotClick, 200)
  } else {
    e.target.textContent = flag ? 'X' : 'O'
    flag = !flag
  }
  checkOrder(e.target.dataset.key)
  checkWin()
}

const robotClick = () => {
  if (isWin.value) return
  const can = divs.value.filter((x) => x.textContent === '')
  const rand = Math.floor(Math.random() * can.length)

  can[rand].textContent = 'O'
  checkOrder(can[rand].dataset.key)
  checkWin()
}

const checkOrder = (key) => {
  clickOrder.value.push(key)

  if (clickOrder.value.length > 6) {
    const remove = clickOrder.value.shift()
    divs.value[remove].textContent = ''
  }
}

const wins = ['123', '456', '789', '147', '258', '369', '159', '357']
const checkWin = () => {
  const o = divs.value
    .filter((x) => x.textContent === 'O')
    .map((x) => x.dataset.key)
    .join('')
  const x = divs.value
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

const divs = ref([]);
const resetGame = () => {
  isWin.value = null
  clickOrder.value = [];
  divs.value.map(x => x.textContent = '');
}
</script>

<template>
  <h1>圈圈差差</h1>
  <h3>畫到第四次時會把第一次畫的地方給清空，增加了思考的空間</h3>
  <div>再說明一次...這不是Bug</div>
  <label for="robotMod">邊緣人模式</label><input type="checkbox" v-model="robotMod" id="robotMod" />
  <h2 v-show="isWin">{{ isWin }} Win!</h2>
  <div class="box">
    <div v-for="i in 9" :key="i" ref="divs" @click="userClick" :data-key="i - 1"></div>
  </div>
  <button class="btn btn-primary mt-3" @click="resetGame">Reset</button>
</template>

<style>
h1 {
  margin-bottom: 10px;
}

h3 {
  margin-top: 0;
  margin-bottom: 5px;
}

.box {
  display: grid;
  width: 400px;
  height: 400px;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: repeat(3, 1fr);
}

.box>div {
  font-size: 48px;
  border: 1px solid #ccc;
  margin: 5px;
  border-radius: 0.375rem;
  display: flex;
  justify-content: center;
  align-items: center;
  transition: 0.5s;
}

button{
  border-radius: 45px;
  background: #121212;
  color:#39f;
  padding: .5rem 3rem;
  border:4px solid #d6d6d7;
  cursor: pointer;
}
</style>
