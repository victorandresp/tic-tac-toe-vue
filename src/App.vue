<script setup>
import { ref, computed } from "vue";
const actualPlayer = ref("X");
const grid = ref([
  ["", "", ""],
  ["", "", ""],
  ["", "", ""],
]);

const CalculateWinner = (flatGrid) => {
  const lines = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6],
  ];
  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i];
    if (
      flatGrid[a] &&
      flatGrid[a] === flatGrid[b] &&
      flatGrid[a] === flatGrid[c]
    ) {
      return flatGrid[a];
    }
  }
  return null;
};
const winner = computed(() => CalculateWinner(grid.value.flat()));
const drawedGame = computed(
  () =>
    grid.value.flat().every((currentValue) => currentValue !== "") &&
    !winner.value
);
const colorPlayerClass = computed(() =>
  actualPlayer.value === "X" ? "x-mark" : "o-mark"
);
const MakeMove = (x, y) => {
  if (winner.value) return;
  if (grid.value[x][y]) return;
  grid.value[x][y] = actualPlayer.value;
  actualPlayer.value = actualPlayer.value === "X" ? "O" : "X";
};
const ResetGame = () => {
  grid.value = [
    ["", "", ""],
    ["", "", ""],
    ["", "", ""],
  ];
  actualPlayer.value = "X";
};
</script>

<template>
  <div class="container">
    <h1>Tic Tac Toe Game Victor Ponce</h1>

    <h3 class="player-text">
      Player <span :class="colorPlayerClass">{{ actualPlayer }}</span> turn
    </h3>

    <div class="column">
      <div v-for="(row, x) in grid" :key="x" class="row">
        <div
          v-for="(cell, y) in row"
          :key="y"
          @click="MakeMove(x, y)"
          :class="`cell ${cell === 'X' ? 'x-mark' : 'o-mark'}`"
        >
          {{ cell }}
          <!-- {{ cell === "X" ? "close" : cell === "O" ? "circle" : "" }} -->
        </div>
      </div>
    </div>

    <div class="text-center">
      <p v-if="drawedGame">draw</p>
      <h2 v-if="winner" class="text-6xl font-bold mb-8">
        Player <span :class="colorPlayerClass">{{ winner }}</span> wins!
      </h2>
      <button @click="ResetGame">Reset</button>
    </div>
  </div>
</template>

<style scoped>
.container {
  text-align: center;
}
.column {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.row {
  display: flex;
}
.cell {
  width: 100px;
  height: 100px;
  border: 1px solid #000;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 30px;
}
.x-mark {
  color: #000;
}
.o-mark {
  color: #e10f0f;
}
</style>
