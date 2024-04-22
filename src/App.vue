<script setup>
import { ref } from 'vue'

const isWin = ref(null);
const wins = ref(['123', '456', '789', '147', '258', '369', '159', '357']);

const userClick = (e) => {
  if (isWin.value) return
  if (e.target.textContent) return
  e.target.textContent = 'X'
  checkWin()
}

const checkWin = () => {
  const divs = [...document.querySelectorAll('.box > div')]
  const o = divs.filter(x => x.textContent === 'O').map(x => x.dataset.key).join('')
  const x = divs.filter(x => x.textContent === 'X').map(x => x.dataset.key).join('')
  for (let win of wins.value) {
    const [a, b, c] = win.split('')
    const check = (n) => n.includes(a) && n.includes(b) && n.includes(c)
    isWin.value = check(o) ? 'O' : check(x) ? 'X' : null
    if (isWin.value) break
  }
}
const resetGame = () => {
  isWin.value = null;
  [...document.querySelectorAll('.box > div')].map(x => x.textContent = '')
}
</script>

<template>
  <div v-show="isWin">{{ isWin }} Win!</div>
  <div class="box d-grid border">
    <div v-for="i in 9" @click="userClick" :data-key="i"
         class="border display-3 d-flex justify-content-center align-items-center"></div>
  </div>
  <button class="btn btn-primary mt-3" @click="resetGame">Reset</button>
</template>

<style>
.box {
  width: 500px;
  height: 500px;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: repeat(3, 1fr);
}
</style>