<script setup lang="ts">
import { Player } from "../model./Player";

interface IDisplayBoardProps {
  players: Player[];
  currentPlayer: Player;
  winner: string | null;
  squares: string[][];
  history: Player[];
}

const props = defineProps<IDisplayBoardProps>();
const emits = defineEmits(["makeMove"]);
</script>
<template>
  <div class="wrapper">
    <h3 class="winnerNotice" v-if="props.winner">
      🎉 Vinnaren är {{ props.currentPlayer.playerName }} 🎉
    </h3>
    <h3 v-if="props.currentPlayer">
      {{ props.currentPlayer.playerName }} spelar
    </h3>
    <div class="board">
      <div class="squares">
        <div v-for="x in 3" :key="x">
          <div
            class="square"
            v-for="y in 3"
            :key="y"
            @click="emits('makeMove', x, y)"
          >
            {{ squares[x - 1][y - 1] }}
          </div>
        </div>
      </div>
    </div>

    <div>
      <h3 v-if="props.history.length > 0">Vinnarhistorik</h3>
      <div
        v-for="(winningPlayer, i) in props.history"
        :key="props.history[i].playerName"
      >
        <p class="winnerPoints">
          {{ i + 1 }}. {{ props.history[i].playerName }}s vunna spel: 
          {{ props.history[i].timesWon }}
        </p>
      </div>
    </div>
  </div>
</template>
<style scoped>
.winnerPoints {
  text-align: start;
}
.square {
  width: 100px;
  height: 100px;
  border: 1px solid black;
  float: left;
  font-size: 4rem;
  text-align: center;
  background-color: #85bdbf;
}

.winnerNotice {
  font-size: 2rem;
  color: rgb(219, 92, 113);
}
</style>
