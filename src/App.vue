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
    <h1>
      Tic Tac Toe Game by
      <a href="https://github.com/victorandresp/" target="_blank">
        @victorandresp
      </a>
    </h1>

    <h2 v-if="!drawedGame && !winner">
      Player <span>{{ actualPlayer }}</span> turn
    </h2>

    <h2 v-if="drawedGame" class="text--secondary">Draw</h2>
    <h2 v-if="winner">
      Player <span class="text--secondary">{{ winner }}</span> wins!
    </h2>

    <div class="table">
      <div class="table_box">
        <div v-for="(row, x) in grid" :key="x" class="row">
          <div
            v-for="(cell, y) in row"
            :key="y"
            @click="MakeMove(x, y)"
            class="cell"
          >
            {{ cell }}
          </div>
        </div>
      </div>
    </div>

    <div class="text-center">
      <button class="button--reset" @click="ResetGame">Reset</button>
    </div>
  </div>
</template>

<style scoped>
.container {
  text-align: center;
}
.table {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.table_box {
  box-shadow: 0px 10px 15px -3px rgba(0, 0, 0, 0.1);
}
.row {
  display: flex;
}
.cell {
  width: 100px;
  height: 100px;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 30px;
  background: var(--primary-color);
}
.row > .cell:nth-child(even),
.row:nth-child(even) > .cell:nth-child(odd) {
  background: var(--secondary-color);
}
.row:nth-child(even) > .cell:nth-child(even) {
  background: var(--primary-color);
}
.button--reset {
  background: var(--secondary-color);
  border: none;
  font-size: 20px;
  padding: 0.5rem;
  margin-top: 1rem;
  cursor: pointer;
}
.button--reset:hover {
  box-shadow: 0px 10px 15px -3px rgba(0, 0, 0, 0.1);
}
.text--secondary {
  color: var(--secondary-color);
}
</style>
