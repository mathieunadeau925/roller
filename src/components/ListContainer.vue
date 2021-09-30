<template>
      <div class="row py-3">
        <div
          v-for="(player, index) in playersList"
          :key="player.id"
          class="col-md-3 col-xs-6 pb-3"
        >
          <div
            :class="getComputedClassBg(index)"
            class="d-flex align-items-center w-100 p-2 rounded mx-auto justify-content-between"
          >
            <div class="font-weight-bold player-name-size">
              {{ player.playerName }}
            </div>
            <div class="ml-3">
              <button
                @click="deletePlayer(player.id)"
                class="btn btn-link btn-lg text-danger no-text-deco"
              >
                X
              </button>
            </div>
          </div>
        </div>
      </div>
</template>

<script>
export default {
  props: {
    playersList: {
      type: Array,
      default: () => []
    }
  },

  methods: {
    getComputedClassBg(index) {
      if (index === 0 || index === 5) {
        return "bg-primary";
      }
      if (index === 1 || index === 6) {
        return "bg-secondary";
      }
      if (index === 2 || index === 7) {
        return "bg-warning";
      }
      if (index === 3) {
        return "bg-success";
      }
      if (index === 4) {
        return "bg-info";
      }

      return "bg-light";
    },
    deletePlayer(playerId) {
      this.$emit("deletePlayer", playerId);
    }
  }
};
</script>

<style>
.no-text-deco {
  text-decoration: none !important;
}

.player-name-size {
  font-size: 1.5rem;
}
</style>