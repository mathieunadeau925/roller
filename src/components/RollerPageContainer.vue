<template>
  <div class="h-100 w-100 bg-dark">
    <div class="container-fluid">
      <div class="row align-items-center justify-content-center">
        <div class="col-12 text-center py-3">
          <h4 class="m-auto text-white">Diablo 2 item roller</h4>
        </div>
      </div>
    </div>
    <div class="container-fluid">
      <AddPlayerInput :playersList="playersList" @updatePlayers="addPlayer" />
      <ListContainer :playersList="playersList" @deletePlayer="deletePlayer" />
      <Roller v-if="playersList.length > 0" :playersList="playersList" />
    </div>
  </div>
</template>

<script>
import AddPlayerInput from "./AddPlayerInput";
import ListContainer from "./ListContainer";
import Roller from "./Roller";

export default {
  components: {
    AddPlayerInput,
    ListContainer,
    Roller
  },
  data() {
    return {
      playersList: []
    };
  },
  methods: {
    addPlayer(newPlayer) {
      this.playersList.push({
        playerName: newPlayer,
        id: `${this.playersList.length + 1}-${newPlayer}`
      });
    },
    deletePlayer(playerId) {
      this.playersList = this.playersList.filter(x => x.id !== playerId);
    }
  }
};
</script>