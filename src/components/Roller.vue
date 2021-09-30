<template>
  <div class="row">
    <div class="col-12">
      <div class="d-flex p-4">
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
      <div
        v-if="hasResult"
        :class="shadowComputed"
        class="fit-content rounded m-auto"
      >
        <div
          class="fit-content p-3 rounded m-auto shadow-text"
          :class="getComputedClassBg(winner.index)"
        >
          <h3>{{ winner.playerName }}</h3>
        </div>
      </div>
      <div v-if="lastWinner.playerName">
        <div>
          <h3>Last winner: {{ lastWinner.playerName }}</h3>
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
      lastWinner: {
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
      },
      effectNumber: 0,
      time: 0
    };
  },
  computed: {
    shadowComputed() {
      return `shadow-${this.effectNumber}`;
    }
  },
  methods: {
    roll() {
      this.hasResult = false;
      this.setAnimateIntervals = false;
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
            if (this.winner.playerName === this.lastWinner.playerName) {
              this.lastWinner = {
                playerName: null,
                playerId: null,
                index: null
              };
              this.animateRoll();
            } else {
              this.hasResult = true;
              this.isRolling = false;
              this.setAnimateIntervals = true;
              this.animateBox();
            }
          }
        });
      }
    },
    getRandomNumber(min, max) {
      const newMin = Math.ceil(min);
      const newMax = Math.floor(max);
      return Math.floor(Math.random() * (newMax - newMin)) + newMin;
    },
    animateBox() {
      if (this.setAnimateIntervals) {
        const interval = setInterval(() => {
          if (!this.setAnimateIntervals || this.time === 60) {
            this.time = 0;
            clearInterval(interval);
            this.effectNumber = null;
            setTimeout(() => {
              this.lastWinner = { ...this.winner };
              this.hasResult = false;
              this.setAnimateIntervals = false;
            }, 1000);
          } else if (this.effectNumber !== 3) {
            this.effectNumber = this.effectNumber + 1;
          } else {
            this.effectNumber = 0;
          }
          this.time += 1;
        }, 50);
      }
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
  text-shadow: 1px 1px 1px rgb(248, 182, 0), 3px 3px 5px blue;
}

.shadow-0 {
  box-shadow: 0 0 20px #fefcc9, 10px -10px 30px #feec85,
    -20px -20px 40px #ffae34, 20px -40px 50px #ec760c, -20px -60px 60px #cd4606,
    0 -80px 70px #973716, 10px -90px 80px #451b0e, 1px 1px 0 yellow,
    2px 2px 0 orange, 3px 3px 0 red;
}
.shadow-1 {
  box-shadow: 0 0 20px #fefcc9, 10px -10px 30px #feec85,
    -20px -20px 40px #ffae34, 20px -40px 50px #ec760c, -20px -60px 60px #cd4606,
    0 -80px 70px #973716, 10px -90px 80px #451b0e, 1px 1px 0 yellow,
    2px 2px 0 orange, 3px 3px 0 red;
}
.shadow-2 {
  box-shadow: 0 0 20px #fefcc9, 10px -10px 30px #feec85,
    -20px -20px 40px #ffae34, 20px -40px 50px #978779, -20px -60px 60px #cd4606,
    0 -80px 70px #973716, 10px -90px 80px #451b0e, 1px 1px 0 rgb(66, 66, 21),
    2px 2px 0 rgb(119, 82, 15), 3px 3px 0 red;
}
.shadow-3 {
  box-shadow: 0 0 20px #fefcc9, 10px -10px 30px #9c8815,
    -20px -20px 40px #2e2a24, 20px -40px 50px #242323, -20px -60px 60px #882b00,
    0 -80px 70px #973716, 10px -90px 80px #451b0e, 1px 1px 0 yellow,
    2px 2px 0 orange, 3px 3px 0 red;
}
.shadow-4 {
  box-shadow: 0 0 20px #fefcc9, 10px -10px 30px #feec85,
    -20px -20px 40px #ffae34, 20px -40px 50px #6d645d, -20px -60px 60px #4d220e,
    0 -80px 70px #973716, 10px -90px 80px #451b0e, 1px 1px 0 yellow,
    2px 2px 0 rgb(68, 46, 4), 3px 3px 0 red;
}
</style>