<script setup lang="ts">
import { ref } from "vue";
import { Player } from "../model./Player";

interface IPlayersProps {
  players: Player[];
}

const playerX = ref("");
const playerO = ref("");

const props = defineProps<IPlayersProps>();
const emits = defineEmits(["addPlayer"]);

const handleSubmit = () => {
  if (playerX.value === "" || playerO.value === "") {
    alert("Fälten får inte vara tomma. Ange namn för båda spelarna");
  } else if (playerO.value === playerX.value) {
    alert("Spelare får inte ha samma namn!");
  } else emits("addPlayer", playerX, playerO);
};
</script>

<template>
  <div class="formContainer" v-if="props.players.length === 0">
    <form class="addForm" @submit.prevent="handleSubmit">
      <label class="xLabel" for="playerX">spelare X</label>
      <input
        class="xInput"
        v-model="playerX"
        id="playerX"
        type="text"
        name="playerName"
      />
      <label class="oLabel" for="playerO">spelare O</label>
      <input
        class="oInput"
        v-model="playerO"
        id="playerO"
        type="text"
        name="playerName"
      />
      <button class="addBtn">Lägg till</button>
    </form>
  </div>
</template>

<style scoped>
.formContainer {
  border-radius: 15px;
  background-color: #929487;
  padding: 50px;
  font-family: Inter, system-ui, Avenir, Helvetica, Arial, sans-serif;
}
.addForm {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.addBtn {
  margin-top: 10px;
}

.xInput,
.oInput {
  border: none;
  height: 20px;
  border-radius: 15px;
}

.xLabel,
.oLabel {
  color: black;
  font-size: 1rem;
  font-weight: 500;
}
</style>
