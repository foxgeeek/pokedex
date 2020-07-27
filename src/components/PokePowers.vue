<template>
  <div class="blackScreen2 largeScreen2">
    <p id="height" class="stats">{{ stats[1].stat.name }} {{ stats[1].base_stat }}</p>
    <p id="weight" class="stats">{{ stats[2].stat.name }} {{ stats[2].base_stat }}</p>
    <p id="region" class="stats">{{ stats[5].stat.name }} {{ stats[5].base_stat }}</p>
    <p id="type" class="stats">{{ stats[0].stat.name }} {{ stats[0].base_stat }}</p>
  </div>
</template>

<script>
export default {
  props: {
    stats: {},
  },
};
</script>

<style>
</style>