<template>
  <div class="blackScreen largeScreen">
    <p id="index">{{ pokedata.number }}</p>
    <div id="typeBubbleDiv">
      <div id="bubble1" class="typeBubble"></div>
      <div id="bubble2" class="typeBubble"></div>
    </div>
    <p id="type">{{ pokedata.type }}</p>
    <p id="height">{{ pokedata.height }} cm</p>
    <p id="weight">{{ pokedata.weight }} kg</p>
    <p id="region">{{ pokedata.region }}</p>
  </div>
</template>

<script>
export default {
  props: {
    pokedata: {},
  },
};
</script>

<style>
</style>