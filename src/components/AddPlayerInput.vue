<template>
  <div class="row">
    <div class="col-12">
      <div class="card bg-secondary text-white text-center">
        <div v-if="hasErrors" class="bg-warning text-secondary p-2">
          This name is already used.
        </div>
        <transition name="fade">
          <div v-if="isEmpty" class="bg-warning text-secondary p-2">
            Please enter a name.
          </div>
        </transition>
        <h5 class="py-2">Enter the player name</h5>
        <div class="row align-items-center justify-content-center pb-2">
          <div class="col-xs-12 col-md-auto p-2 input-group fit-content">
            <input class="form-control" type="text" v-model="playerName" v-on:keyup.enter="addPlayer" />
          </div>
          <div class="col-xs-12 col-md-auto pl-2">
            <button class="btn btn-primary" @click="addPlayer">
              Add player
            </button>
          </div>
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
  data() {
    return {
      hasErrors: false,
      playerName: null,
      isEmpty: false
    };
  },
  methods: {
    addPlayer() {
      this.hasErrors = false;
      this.isEmpty = false;
      if (this.playersList.some(x => x.playerName === this.playerName)) {
        this.hasErrors = true;
      } else if (!this.playerName) {
        this.isEmpty = true;
      } else {
        this.$emit("updatePlayers", this.playerName);
        this.playerName = '';
      }
    }
  }
};
</script>

<style>
.fit-content {
  width: fit-content !important;
}
</style>