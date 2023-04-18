<script setup lang="ts">
import { Player } from "../model./Player";

interface IWinnersProps {
  history: Player[];
}

const props = defineProps<IWinnersProps>();

const emits = defineEmits(["clearWinnerHistory", "clearPlayers", "reset"]);
</script>
<template>
  <div class="buttonContainer">
    <button class="clearPlayers" @click="emits('clearPlayers')">
      Rensa spelare
    </button>
    <button class="restartBtn" @click="emits('reset')">Börja om</button>
    <button
      v-if="props.history.length !== 0"
      class="clearWinners"
      @click="emits('clearWinnerHistory')"
    >
      Rensa Vinnarhistorik
    </button>
  </div>
</template>
<style scoped>
.buttonContainer {
  display: flex;
  flex-direction: row;
  justify-content: center;
  margin: 30px;
}

button {
  background-color: #29524a;
  color: white;
  margin: 10px;
}

button:hover {
  background-color: #87bba2;
}
</style>
