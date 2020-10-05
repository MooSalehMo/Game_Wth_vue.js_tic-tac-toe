<template>
  <div>
    <div class="gameStatus" :class="gameStatusColor">{{ gameStatusMessage }}</div>

    <div class="grid">
      <Cell v-for="n in 9" :key="n" :marker="n" />
    </div>
  </div>
</template>

<script>
import EventBus from "../eventBus.js";
import Cell from "./Cell";

export default {
  name: "grid",
  components: {
    Cell,
  },
  computed: {
    noneActivePlayer() {
      if (this.activePlyer === "o") {
        return "x";
      }
      return "o";
    },
  },
  methods: {
    changePlayer() {
      this.activePlyer = this.noneActivePlayer;
      this.gameStatusMessage = `${this.activePlyer}'s Turn `;
    },
    checkForWin() {
      let cells = this.cells;
      return this.winContitions.some((condtion) => {
        let count = 0;
        condtion.forEach((cell) => {
          if (cells[cell] === this.activePlyer) {
            count++;
          }
        });
        if (count === 3) {
          return true;
        }
        return false;
      });
    },
    gameIsWon() {
      EventBus.$emit("win", this.activePlyer);
      this.gameStatusMessage = `${this.activePlyer} Wins`;
      EventBus.$emit("freeze");
      return "win";
    },
    changeGameStatus() {
      if (this.checkForWin()) {
        return this.gameIsWon();
      } else if (this.moves === 9) {
        return "draw";
      }
      this.changePlayer();
      return "turn";
    },
  },
  created() {
    EventBus.$on("shot", (callNumber) => {
      this.cells[callNumber] = this.activePlyer;
      this.moves++;
      this.gameStatus = this.changeGameStatus();
    });
    EventBus.$on("resetGrid", () => {
      Object.assign(this.$data, this.$options.data());
    });
  },
  data() {
    return {
      activePlyer: "o",
      gameStatus: "turn",
      gameStatusMessage: "o's turn",
      gameStatusColor: "statusTurn",
      moves: 0,
      winContitions: [
        [1, 2, 3],
        [4, 5, 6],
        [7, 8, 9], // rows
        [1, 3, 7],
        [2, 5, 8],
        [3, 6, 9], // columns
        [1, 5, 9],
        [3, 5, 7],
        [3, 6, 9], // diagonals
      ],
      cells: {
        1: "",
        2: "",
        3: "",
        4: "",
        5: "",
        6: "",
        7: "",
        8: "",
        9: "",
      },
    };
  },
  watch: {
    gameStatus() {
      if (this.gameStatus === "win") {
        this.gameStatusColor = "statusWin";
      } else if (this.gameStatus === "draw") {
        this.gameStatusColor = "statusDraw";
        this.gameStatusMessage = "there s no winner";
      }
    },
  },
};
</script>

<style lang="css">
.grid {
  display: grid;
  grid-template-columns: 100px 100px 100px;
  grid-gap: 10px;
  color: #fff;
  background-color: #333;
  padding: 10px;
  width: 100%;
  position: relative;
  margin: auto;
}

.gameStatus {
  padding: 20px;
  font-weight: bold;
  font-size: 2em;
  color: #fff;
  border-top-left-radius: 30px;
  border-top-right-radius: 30px;
  text-transform: uppercase;
}

.statusTurn {
  background-color: #aac405;
}
.statusWin {
  background-color: #009688;
}
.statusDraw {
  background-color: #d7162e;
}
</style>
