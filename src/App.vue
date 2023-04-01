<script setup>
import {ref,computed} from 'vue'

let value = 0;
const player = ref('X');
const board = ref([
  ['','',''],
  ['','',''],
  ['','',''],
])

const calculateWinner = (squares) => {
  const lines = [
    //all possibilites
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
    if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
      //winner
      return squares[a];
    }
  }
  return null;
}

//calling an calculate winner
const winner = computed(()=>calculateWinner(board.value.flat()))

const Makemove = (x,y)=>{
  

  if(winner.value) return

  //if already places character then return bcz, we can't place in that place
  if(board.value[x][y] !=='') return

  //else we just placing 
  board.value[x][y] = player.value

  //moving to next player
  value++;
  player.value = player.value === 'X'?'O':'X'
}


const Reset = () =>{
  value=0;
  board.value = [
    ['','',''],
    ['','',''],
    ['','',''],
  ]
  player.value = 'X'
}

</script>

<template>
  <main class="min-h-screen pt-8 text-center">
    <h1 class="mb-8 text-3xl font-bold uppercase">Tic Tac Toe</h1>
    
    <h3 class="mb-4 text-xl">Player {{ player }}'s turn</h3>

    <div class="flex flex-col items-center mb-8">
    
      <div
        v-for="(row,x) in board"
        :key="x"
        class="flex">
        
        <div 
          v-for="(cell,y) in row"
          :key="y"
          @click="Makemove(x,y)"
          :class="`border border-white w-20 h-20 hover:bg-gray-700 flex 
          items-center justify-center material-icons-outlined text-4xl 
          cursor-pointer ${cell === 'X' ?'text-pink-500':'text-blue-400'}`">
          {{ cell==='X'?'close':cell==='O'?'circle':'' }}
        </div>
      
      </div>

      
    </div>
    
    <h2 v-if="winner" class="mb-8 text-6xl font-bold">Player '{{ winner }}' wins!</h2>
    <h2 v-if="value===9 && !winner" class="mb-8 text-6xl font-bold">Match draw</h2>
  
    <button @click="Reset" class="px-4 py-2 font-bold uppercase duration-300 bg-pink-500 rounded hover:bg-pink-600">RESET GAME</button>
  
  </main>
</template>

<style>
body {
	@apply bg-gray-800 text-white;
}
</style>
