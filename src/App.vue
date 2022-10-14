<script setup>
  import { ref, computed } from "vue";

  const player = ref("X")
  const board = ref([
    ["", "", ""],
    ["", "", ""],
    ["", "", ""]
  ])

  const calculateWinner = (square) => {
    const lines = [
      [0,1,2],
      [3,4,5],
      [6,7,8],
      [0,3,6],
      [1,4,7],
      [2,5,8],
      [0,4,8],
      [2,4,6],
    ]

    for (let i = 0; i < lines.length; i++) {
      const [a,b,c] = lines[i]
      
      if (square[a] && square[a] === square[b] && square[a] === square[c]) {
        return square[a]
      }
    }
    return null
  }

  const winner = computed(() => calculateWinner(board.value.flat()))
  
  const makeMove = (x,y)=> {
    if (winner.value) return

    if (board.value[x][y] !== '') return

    board.value[x][y] = player.value

    player.value = player.value === 'X' ? 'O' : 'X'
  }

  const resetGame = ()=> {
      board.value = [
      ["", "", ""],
      ["", "", ""],
      ["", "", ""]
    ]

    player.value = player.value[Math.floor(Math.random())]
    
  }

</script>

<template>
  <div class="py-10 text-center dark:bg-slate-800 dark:text-white min-h-screen">
    <h1 class="font-bold text-4xl mb-8">TİK TAK TOE</h1>
    <h3 class="font-bold text-3xl mb-8">Player {{player}}'s turn </h3>
    <div class="flex flex-col items-center">
      <div v-for="(row,x) in board" :key="x" class="flex">
        <div 
        v-for="(cell,y) in row" :key="y" 
        @click="makeMove(x,y)"
        :class="`border border-white w-20 h-20 hover:bg-gray-400 flex items-center justify-center cursor-pointer text-4xl ${cell === 'X' ? 'text-pink-400' : 'text-blue-400'}`">
          {{cell}}
        </div>     
      </div>

      <h3 v-if="winner" class="text-3xl my-10">Player {{winner}} wins!!!</h3>
      <button @click="resetGame" class="px-5 py-3 bg-red-300 rounded-md my-10">Reset Game</button>
    </div>
   
  </div>
</template>
