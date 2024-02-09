<script setup>
import {ref, computed} from "vue";

const player = ref('X');

const board = ref([ 
  ['', '', ''],
  ['', '', ''],
  ['', '', '']
]);

const MakeMove = (x, y) => {
  if (winner.value) return;
  if (board.value[x][y]) return

  board.value[x][y] = player.value
  player.value = player.value === 'X' ? 'O' : 'X'

};

const CalculateWinner = (board) => {
  const lines = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6]
  ];

  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i];
    if (board[a] && board[a] === board[b] && board[a] === board[c]) {
      console.log(a, b, c);
      return board[a]
      
    }
  }
  return null;

};

const winner = computed(() => CalculateWinner(board.value.flat()));

const ResetGame = () => {
  board.value = [
    ['', '', ''],
    ['', '', ''],
    ['', '', '']
  ],
  player.value = 'X'
}

</script>

<template>
  <main class="text-center pt-8 dark:bg-gray-800 text-white">
    <h1 class="text-3xl font-bold mb-8">Tic Tac Toe</h1>
    <h3 class="text-xl mb-8">Player {{ player }}'s turn</h3>

    <div class="flex flex-col items-center mb-8">
      <div
        v-for="(row, x) in board"
        :key="x"
        class="flex"
      >

      <div
        v-for="(cell, y) in row"
        :key="y"
        @click="MakeMove(x, y)"
        :class="`border border-white h-24 w-24 text-4xl flex items-center justify-center cursor-pointer hover:bg-gray-600
        ${cell === 'X' ? 'text-pink-500' : 'text-blue-500'}`"
      >
      {{ cell === 'X' ? 'X' : cell === 'O' ? 'O' : '' }}

      </div>

      </div>

    </div>
    <div v-if="winner">
    <h1 class="text-4xl mb-4">Player {{ winner }} wins!</h1>
  </div>

  <button @click="ResetGame" class="bg-pink-500 hover:bg-pink-800 px-4 py-2 rounded">Reset</button>

  </main>

</template>

<style >

body{
  @apply bg-gray-800 text-white
}
</style>
