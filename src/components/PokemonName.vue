<template>
  <div class="left-commands">
    <div id="powerBtn" class="noirBtn0 btn large" @click="powerOff"></div>
    <div class="commands-central">
      <div class="longBtn redBtn"></div>
      <div class="longBtn blueBtn"></div>
      <div class="greenScreen medlargeScreen">
        <h4 v-if="!power" class="title title-4" id="name">{{ name }}</h4>
      </div>
    </div>
    <div class="croix"></div>
  </div>
</template>

<script>
import { EventBus } from "../plugins/event-bus";
export default {
  data() {
    return {
      power: false,
    }
  },
  props: {
    name: { type: String, required: true },
  },
  methods: {
    async powerOff() {
      this.power = !this.power
      const data = {
        off: this.power,
        info: 'off'
      }
      !this.power ? document.getElementById("powerBtn").classList.remove("noirBtn2") : document.getElementById("powerBtn").classList.add("noirBtn2")
      !this.power ? document.getElementById("blueBtn").classList.remove("noirBtn2") : document.getElementById("blueBtn").classList.add("noirBtn2")
      await EventBus.$emit("powerOff", data);
    },
  },
};
</script>

<style>
</style>