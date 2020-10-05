<template>
  <div id="app">
    <Header />

    <h3 class="statistic">
      <span class="ptayerName">PX</span> has
      <span class="results">{{ wins["x"] }}</span> wins | match
      <span class="results">{{ matchs + 1 }}</span> |
      <span class="ptayerName">PO</span> has
      <span class="results">{{ wins["o"] }}</span> wins
    </h3>

    <Grid />

    <button class="restart" @click="restart">restart</button>
  </div>
</template>

<script>
import Header from "./components/Header";
import Grid from "./components/Grid";
import EventBus from "./eventBus";

export default {
  name: "App",
  components: {
    Header,
    Grid,
  },
  data() {
    return {
      matchs: 0,
      wins: {
        o: 0,
        x: 0,
      },
    };
  },
  methods: {
    restart() {
      EventBus.$emit("clearCells");
      EventBus.$emit("resetGrid");
      this.matchs++;
    },
  },
  created() {
    EventBus.$on("win", (winner) => this.wins[winner]++);
  },
};
</script>

<style lang="css">
#app {
  font-family: "Dosis", sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 0 auto;
  max-width: 340px;
}

.restart {
  background-color: #f00;
  color: #fff;
  font-size: 2em;
  font-weight: bold;
  padding: 20px;
  width: 100%;
  border: none;
  outline: none;
  cursor: pointer;
  text-transform:upperCase;
  border-bottom-right-radius: 30px;
  border-bottom-left-radius: 30px;
}
.statistic {
  font-weight: 800;
  text-transform: capitalize;
}
.statistic .ptayerName {
  font-weight: 800;
  color: #fff;
  background-color: #aac405;
  padding: 2px 3px;
  border-radius: 5px;
}
.statistic .results {
  font-weight: 800;
  text-decoration: underline;
  color: #fff;
  padding: 1px 5px;
  border-radius: 5px;
  background-color: #009688;
}
</style>
