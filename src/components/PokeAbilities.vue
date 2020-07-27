<template>
  <div class="blackScreen3 largeScreen3">
    <p v-if="abilities[0] && !infos" id="ability1" class="stats" @click="getAbility(abilities[0].ability.url)">#1 {{ abilities[0].ability.name }}</p>
    <p v-if="abilities[1] && !infos" id="ability2" class="stats" @click="getAbility(abilities[1].ability.url)">#2 {{ abilities[1].ability.name }}</p>
    <div v-else>
      <div class="stats-description" v-for="effect in effects.effect_entries" :key="effect.id">
        <p v-if="effect.language.name === 'en'">{{ effect.effect }}</p>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      infos: false,
      effects: {}
    }
  },
  props: {
    abilities: {},
  },
  created() {
    this.infos = false
  },
  watch: {
    abilities: {
      handler() {
        this.infos = false;
      }
    }
  },
  methods: {
    async getAbility(url) {
      await axios
        .get(url)
        .then((res) => {
          this.effects = res.data;
          this.infos = true;
        });
    },
  },
};
</script>

<style>
</style>