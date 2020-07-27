<template>
  <div class="gridBtn blueGrid">
    <div class="gridCell" @click="showDetails"></div>
    <div class="gridCell" @click="showPowers"></div>
    <div class="gridCell" @click="showAbilities"></div>
    <div class="gridCell disabled"></div>
    <div class="gridCell disabled"></div>
    <div class="gridCell disabled"></div>
    <div class="gridCell disabled"></div>
    <div class="gridCell disabled"></div>
    <div class="gridCell disabled"></div>
    <div class="gridCell disabled"></div>
  </div>
</template>

<script>
import { EventBus } from "../plugins/event-bus";
export default {
  data() {
    return {
      off: false,
    };
  },
  methods: {
    showDetails() {
      !this.off ? EventBus.$emit("getInfos", "details") : "";
    },
    showPowers() {
      !this.off ? EventBus.$emit("getInfos", "powers") : "";
    },
    showAbilities() {
      !this.off ? EventBus.$emit("getInfos", "abilities") : "";
    },
  },
  created() {
    this.off = false;
    EventBus.$on("powerOff", (data) => {
      this.off = data.off;
      data.off ? (this.info = data.info) : (this.info = "details");
    });
  },
};
</script>

<style>
.gridCell.disabled:hover {
  cursor: auto;
  transform: translateY(0rem);
}
.gridCell.disabled {
  background-color: darkslategray;
}
</style>