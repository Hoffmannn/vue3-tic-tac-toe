<script setup lang="ts">
import { ref } from 'vue';
import BoardSquare from './components/BoardSquare.vue';

const boardInitialValues = Array(9).fill(null)

const board = ref(
  [...boardInitialValues])

const victoryIndexes = [
  [0, 1, 2],
  [3, 4, 5],
  [6, 7, 8],

  [0, 3, 6],
  [1, 4, 7],
  [2, 5, 8],

  [0, 4, 8],
  [2, 4, 6]
];

const turn = ref<'X' | 'O'>('X');

const winner = ref<'X' | 'O' | null>(null);
const draw = ref(false);

function resetBoard() {
  board.value = [...boardInitialValues];
  winner.value = null;
  turn.value = 'X';
  draw.value = false;
}

function calculateVictory(player: 'X' | 'O') {
  for (const pos of victoryIndexes) {
    let points = 0
    for (const index of pos) {
      if (board.value[index] === player) {
        points += 1
      }
    }
    if (points === 3) {
      winner.value = player;
      break;
    }
  }
  if (board.value.every((value) => value !== null) && !winner.value) {
    console.log('draw')
    draw.value = true;
  }
}

function handleClick(index: number) {
  if (board.value[index] === null && !winner.value) {
    board.value[index] = turn.value
    calculateVictory(turn.value);
    turn.value = turn.value === 'X' ? 'O' : 'X';
  }

}


</script>

<template>
  <main>
    <h1>Tic-Tac-Toe</h1>
    <div class="board">

      <div class="row top">
        <BoardSquare :value="board[0]" :handleClick="() => handleClick(0)" />
        <BoardSquare :value="board[1]" :handleClick="() => handleClick(1)" />
        <BoardSquare :value="board[2]" :handleClick="() => handleClick(2)" />
      </div>
      <div class="row">
        <BoardSquare :value="board[3]" :handleClick="() => handleClick(3)" />
        <BoardSquare :value="board[4]" :handleClick="() => handleClick(4)" />
        <BoardSquare :value="board[5]" :handleClick="() => handleClick(5)" />
      </div>
      <div class="row bottom">
        <BoardSquare :value="board[6]" :handleClick="() => handleClick(6)" />
        <BoardSquare :value="board[7]" :handleClick="() => handleClick(7)" />
        <BoardSquare :value="board[8]" :handleClick="() => handleClick(8)" />
      </div>

    </div>

    <div v-if="winner">
      <h2>{{ winner }} wins!</h2>
    </div>
    <div v-if="draw">
      <h2>It's a draw!</h2>
    </div>

    <button v-on:click="resetBoard()">Restart game</button>
  </main>
  <footer>
    <p>
      Made by <a href="https://github.com/Hoffmannn" target="_blank">Lucas Hoffmann</a>.
    </p>
    <p>
      <a href="https://github.com/Hoffmannn/vue3-tic-tac-toe" target="_blank">Source code</a>.
    </p>
    <p>
      {{ new Date().getFullYear() }}.
    </p>
  </footer>
</template>

<style>
main {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-start;
  gap: 10px;
}

.row {
  display: flex;
  border: 1px solid white;

  &>:not(:last-child) {
    border-right: 1px solid;
  }


}

.top {
  border-top-left-radius: 10px;
  border-top-right-radius: 10px;
  border-bottom: none;
}

.bottom {
  border-bottom-left-radius: 10px;
  border-bottom-right-radius: 10px;
  border-top: none;
}

button {
  margin-top: 20px;
  padding: 10px;
  font-size: 1.2rem;
  border-radius: 5px;
  background-color: #4CAF50;
  color: white;
  border: none;
  cursor: pointer;

  &:hover {
    background-color: #3E8E41;
  }
}
</style>
