<script setup>
import { reactive, ref } from "vue";
let player1 = reactive({
  id: 1,
  name: "player1",
  value: "X",
  boxes1: [],
});
let player2 = reactive({
  id: 2,
  name: "player2",
  value: "O",
  boxes2: [],
});
let boxes = reactive(["", "", "", "", "", "", "", "", ""]);
const winnerState = [
  [0, 1, 2],
  [3, 4, 5],
  [6, 7, 8],
  [0, 3, 6],
  [1, 4, 7],
  [2, 5, 8],
  [0, 4, 8],
  [2, 4, 6],
];

let active = ref(true);
let currentPlayer = ref(player1);
let winner = ref(null);

const checkWinner = () => {
  const currentBoxes =
    currentPlayer.value === player1 ? player1.boxes1 : player2.boxes2;
  for (const i of winnerState) {
    if (i.every((index) => currentBoxes.includes(index))) {
      winner.value = currentPlayer.value.name;
      return true;
    }
  }
  return false;
};

const setActive = (index) => {
  if (boxes[index] === "" && !winner.value) {
    boxes[index] = currentPlayer.value.value; //value means 'o' or 'x'
    if (currentPlayer.value === player1) {
      player1.boxes1.push(index);
      if (checkWinner()) return;
      currentPlayer.value = player2;
    } else {
      player2.boxes2.push(index);
      if (checkWinner()) return;
      currentPlayer.value = player1;
    }
    active.value = !active.value;
  }
};

const resetAll = () => {
    
  if (winner.value!==null) {
    player1.boxes1 = [];
    player2.boxes2 = [];
    boxes= ["", "", "", "", "", "", "", "", ""];
    active.value = true;
    currentPlayer.value = player1;
    winner.value = null;
  }
};
</script>

<template>
  <div id="container">
    <h1>Tic Tac Toe Game</h1>
    <div>
      <button :disabled="!active">Player1 (X)</button>
      <button :disabled="active">Player2 (O)</button>
    </div>
    <table>
      <tr>
        <td @click="setActive(0)">{{ boxes[0] }}</td>
        <td @click="setActive(1)">{{ boxes[1] }}</td>
        <td @click="setActive(2)">{{ boxes[2] }}</td>
      </tr>
      <tr>
        <td @click="setActive(3)">{{ boxes[3] }}</td>
        <td @click="setActive(4)">{{ boxes[4] }}</td>
        <td @click="setActive(5)">{{ boxes[5] }}</td>
      </tr>
      <tr>
        <td @click="setActive(6)">{{ boxes[6] }}</td>
        <td @click="setActive(7)">{{ boxes[7] }}</td>
        <td @click="setActive(8)">{{ boxes[8] }}</td>
      </tr>
    </table>

    <div v-if="winner">
      <h2>{{ winner }} Wins!</h2>
      <button id="reset" @click="resetAll">Reset</button>
    </div>
  </div>
</template>

<style>
#reset {
  background-color: rgb(223, 40, 8);
  font-size: larger;
  margin-bottom: 10px;
  margin-top: 10px;
}


#container {
  background-color: blue;
  width: 600px;
  box-shadow: 1px 1px 2px 2px rgb(139, 221, 8);
}
h1 {
  padding-top: 10px;
  padding-bottom: 10px;
  text-align: center;
  background-color: rgb(180, 121, 43);
}
table {
  margin-left: auto;
  margin-right: auto;
  padding-bottom: 20px;
}
table td {
  width: 100px;
  height: 100px;
  border: 1px solid;
  text-align: center;
  font-size: 60px;
  background-color: rgb(180, 153, 117);
  color: brown;
}
h2 {
  padding-bottom: 20px;
}
button {
  margin-bottom: 20px;
  margin-right: 20px;
  font-size: x-large;
  margin-bottom: 30px;
}
td:active {
  background-color: rgb(15, 197, 182);
}
</style>
