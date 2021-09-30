<template>
  <div class="row">
    <div class="col-12">
      <div class="d-flex py-4">
        <div class="mx-auto">
          <button @click="roll" class="btn btn-primary btn-lg btn-danger">
            ROLL!
          </button>
        </div>
      </div>
    </div>
    <div class="col-12 text-white">
      <div v-if="isRolling" class="m-auto w-100">
        <div
          v-if="playerBeforeCurrent && playerBeforeCurrent.playerName"
          class="rounded text-center"
          :class="getComputedClassBg(playerBeforeCurrent.index)"
        >
          <h3>{{ playerBeforeCurrent.playerName }}</h3>
        </div>
        <h4
          :class="getComputedClassBg(winner.index)"
          class="rounded py-2 text-center"
        >
          {{ winner.playerName }}
        </h4>
        <div
          v-if="playerAfterCurrent && playerAfterCurrent.playerName"
          class="rounded text-center"
          :class="getComputedClassBg(playerAfterCurrent.index)"
        >
          <h3>{{ playerAfterCurrent.playerName }}</h3>
        </div>
      </div>
      <div v-if="hasResult" class="w-100 pt-4">
        <div
          class="fit-content p-3 rounded m-auto shadow-text"
          :class="getComputedClassBg(winner.index)"
        >
          <h3>{{ winner.playerName }}</h3>
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
      required: true
    }
  },
  data() {
    return {
      isRolling: false,
      hasResult: false,
      winner: {
        playerName: null,
        playerId: null,
        index: null
      },
      playerBeforeCurrent: {
        playerName: null,
        playerId: null,
        index: null
      },
      playerAfterCurrent: {
        playerName: null,
        playerId: null,
        index: null
      }
    };
  },
  methods: {
    roll() {
      this.hasResult = false;
      this.isRolling = true;
      this.animateRoll();
    },
    async animateRoll() {
      for (let i = 0; i <= 30; i++) {
        await new Promise(res => {
          setTimeout(() => {
            this.winner = this.playersList[
              this.getRandomNumber(0, this.playersList.length)
            ];
            this.winner = {
              ...this.winner,
              index: this.playersList.findIndex(x => x.id === this.winner.id)
            };
            this.playerBeforeCurrent = this.setPlayerBeforeCurrent();
            this.playerAfterCurrent = this.setPlayerAfterCurrent();
            res();
          }, 150);
          if (i === 30) {
            this.hasResult = true;
            this.isRolling = false;
          }
        });
      }
    },
    getRandomNumber(min, max) {
      const newMin = Math.ceil(min);
      const newMax = Math.floor(max);
      return Math.floor(Math.random() * (newMax - newMin)) + newMin;
    },
    setPlayerBeforeCurrent() {
      const currentIndex = this.playersList.findIndex(
        x => x.id === this.winner.id
      );
      if (currentIndex > 0) {
        return {
          ...this.playersList[currentIndex - 1],
          index: currentIndex - 1
        };
      } else {
        return {
          ...this.playersList[currentIndex + 1],
          index: currentIndex + 1
        };
      }
    },
    setPlayerAfterCurrent() {
      const currentIndex = this.playersList.findIndex(
        x => x.id === this.winner.id
      );
      if (currentIndex < 0) {
        return {
          ...this.playersList[currentIndex + 1],
          index: currentIndex + 1
        };
      } else {
        return {
          ...this.playersList[currentIndex - 1],
          index: currentIndex - 1
        };
      }
    },
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
    }
  }
};
</script>

<style>
.shadow-text {
  text-shadow: 1px 1px 1px #000, 3px 3px 5px blue;
}
</style>