<script setup lang="ts">
import { computed, ref, watch, onMounted } from "vue";
import { Player } from "../model./Player";
import AddPlayers from "./AddPlayers.vue";
import DisplayBoard from "./DisplayBoard.vue";
import ClearButtons from "./ClearButtons.vue";

let players = ref<Player[]>([]);
const currentPlayer = ref<Player>(players.value[0]);

const squares = ref([
  ["", "", ""],
  ["", "", ""],
  ["", "", ""],
]);

const calculateWinner = (squares: string[]) => {
  const winningCombinations = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6],
  ];

  for (let i = 0; i < winningCombinations.length; i++) {
    const [a, b, c] = winningCombinations[i];
    if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
      return squares[a];
    }
  }
  return null;
};
const winner = computed(() => calculateWinner(squares.value.flat()));

const isBoardFull = (squares: string[][]) => {
  for (let x = 0; x < squares.length; x++) {
    for (let y = 0; y < squares.length; y++) {
      if (!squares[x][y]) {
        return false;
      }
    }
  }
  return true;
};

const makeMove = (x: number, y: number) => {
  if (winner.value || squares.value[x - 1][y - 1]) {
    return alert("Inga fler klick här!");
  }

  squares.value[x - 1][y - 1] = currentPlayer.value.playerValue;

  if (!winner.value) {
    currentPlayer.value =
      currentPlayer.value === players.value[0]
        ? players.value[1]
        : players.value[0];
  }

  if (isBoardFull(squares.value) && !winner.value)
    return alert("Spelet är över. Ingen vann!");
};

const reset = () => {
  squares.value = [
    ["", "", ""],
    ["", "", ""],
    ["", "", ""],
  ];
};

const clearPlayers = () => {
  localStorage.removeItem("players");
  window.location.reload();
  reset();
};

const clearWinnerHistory = () => {
  localStorage.removeItem("history");
  window.location.reload();
};

const addPlayer = (playerNameX: string, playerNameO: string) => {
    players.value.push(
    new Player(playerNameX, "X", 1),
    new Player(playerNameO, "O", 1)
  );
  localStorage.setItem("players", JSON.stringify(players.value));
  window.location.reload();
  
};

const history = ref<Player[]>([]);
watch(winner, (current, previous) => {
  if (current && !previous) {
    const winningPlayer: Player = currentPlayer.value;
    const positionInHistory = history.value.findIndex(
      (player) => player.playerName == winningPlayer.playerName
    );
    if (positionInHistory > -1) {
      history.value[positionInHistory].timesWon++;
    } else {
      history.value.push(winningPlayer);
    }
    localStorage.setItem("history", JSON.stringify(history.value));
  }
});

onMounted(() => {
  history.value = JSON.parse(localStorage.getItem("history") || "[]");
  const randomIndex = Math.floor(Math.random() * players.value.length);
  currentPlayer.value = players.value[randomIndex];
});
players = ref(JSON.parse(localStorage.getItem("players") || "[]"));
</script>

<template>
  <h1>Robin's Tic Tac Toe</h1>
  <AddPlayers @add-player="addPlayer" :players="players"></AddPlayers>
  <DisplayBoard
    v-if="players.length !== 0"
    :players="players"
    :winner="winner"
    :current-player="currentPlayer"
    :squares="squares"
    :history="history"
    @make-move="makeMove"
  ></DisplayBoard>
  <ClearButtons
    :history="history"
    v-if="players.length !== 0"
    @clear-players="clearPlayers"
    @reset="reset"
    @clear-winner-history="clearWinnerHistory"
  ></ClearButtons>
</template>

<style scoped></style>
