<script setup>
import { ref, reactive  } from 'vue'

const defaultMatrice = [
  [0, 0, 0],
  [0, 0, 0],
  [0, 0, 0]
]

const players = {
  0: { playValue: -1, playDisplay: '0'},
  1: { playValue: 1, playDisplay: 'X'}
}

let game = reactive({round: 0, playground: defaultMatrice, isEnd: false, message: ''});


const reset = () => {
  game = {round: 0, playground: defaultMatrice, message: ''};
  console.log(game)
}

const printValue = (n) => {
  if (n === players[0].playValue) return players[0].playDisplay
  if (n === players[1].playValue) return players[1].playDisplay
  return ''
}

const checkPlay = (x, y) => {
  if(game.playground[y][x] === 0) {
    game.round++;
    game.playground[y][x] = players[game.round % 2].playValue;
    isGameEnd()
  }
}

const isGameEnd = () => {
  if(game.round === 9) {
    game.message = 'No winner'
    game.isEnd = true;
  }

  let winOptions = [
    [0,1,2],
    [3,4,5],
    [6,7,8],
    [0,3,6],
    [1,4,7],
    [2,5,8],
    [0,4,8],
    [2,4,6]
  ]
  let playground = game.playground.flat()
  let hasWinner = winOptions.find((line) => {
    let count = playground[line[0]] + playground[line[1]] + playground[line[2]]
    return count === 3 || count === -3
  })
  if(hasWinner){
    game.message = 'Winner is player' + playground[hasWinner[0]]
    game.isEnd = true;
  }
}

const playerPlay = (place) => {
  let x = place % 3
  let y = (place - x) / 3;
  checkPlay(x, y)
}

</script>

<template>
  <div class="box-container">
    <div class="box" v-for="(box, index) in game.playground.flat()" @click="playerPlay(index)">{{ printValue(box) }}
    </div>
  </div>
  <p>{{ game.message }}</p>
  <button @click="reset()">Reset</button>
</template>

<style scoped>
.box-container {
  background-color: black;
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-gap: 5px;
}

.box {
  height: 50px;
  width: 50px;
  background-color: white;
  color: black;
  text-align: center;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 2rem;
}
</style>
