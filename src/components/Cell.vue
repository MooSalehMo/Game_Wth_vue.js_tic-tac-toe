<template>
  <div class="cell" @click="shot">{{ mark }}</div>
</template>

<script>
import EventBus from "../eventBus.js";
export default {
  name: "cell",
  props: ["marker"],
  data() {
    return {
      mark: "",
      canPlaceMark: true,
    };
  },
  methods: {
    shot() {
      if (this.canPlaceMark) {
        this.mark = this.$parent.activePlyer;
        this.canPlaceMark = false;
        EventBus.$emit("shot", this.marker);
      }
    },
  },
  created() {
    EventBus.$on("freeze", () => (this.canPlaceMark = false));
    EventBus.$on("clearCells", () => {
      this.mark = "";
      this.canPlaceMark = true;
    });
  },
};
</script>

<style lang="css">
.cell {
  height: 100px;
  line-height: 100px;
  font-size: 3.5em;
  background-color: #222;
  cursor: pointer;
  transition: all 0.2s ease-in-out;
}
.cell:hover {
  background-color: #444;
}
</style> 