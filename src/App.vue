<template>
  <div id="app">
    <div id="bgOverlay"></div>
    <Pokedex />
  </div>
</template>

<script>
import Pokedex from "./components/Pokedex";
import { EventBus } from "./plugins/event-bus";

export default {
  name: "App",
  components: { Pokedex },
  destroyed() {
    EventBus.$off();
  },
};
</script>

<style lang="scss">
@import "./assets/css/main.scss";
</style>
