<template>
  <div class="pokedex">
    <div class="pokedex">
      <div class="left">
        <div class="left-main">
          <PokedexTop />
          <div class="left-screen">
            <div class="screen-border-grey">
              <ScreenBtnsTop />
              <PowerLeft v-if="off && info === 'off'" />
              <PokemonImage v-if="!off" @pokemonDetails="getDetails($event)" :index="index" />
              <ScreenBtnsBottom />
            </div>
          </div>
          <PokemonName :name="name" />
        </div>
        <CynlinderCenter />
      </div>
      <div class="right-wrapper">
        <div class="right">
          <PowerRight v-if="off && info === 'off'" />
          <ScreenRight :pokedata="pokedata" v-if="!off && info === 'details'" />
          <PokePowers :stats="stats" v-if="!off && info === 'powers'" />
          <PokeAbilities :abilities="abilities" v-if="!off && info === 'abilities'" />
          <PokedexButtons />
          <div class="longBtnRight">
            <div class="longBtn noirBtn"></div>
            <div class="longBtn noirBtn"></div>
          </div>
          <div class="jauneBtn btn large" id="btnJaune"></div>
          <div class="noirScreen mediumScreen" id="prev" @click="leftItem">{{ left }}</div>
          <div class="noirScreen mediumScreen" id="next" @click="rightItem">{{ right }}</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import PokemonImage from "./PokemonImage";
import PowerLeft from "./PowerLeft";
import PowerRight from "./PowerRight";
import PokePowers from "./PokePowers";
import PokeAbilities from "./PokeAbilities";
import CynlinderCenter from "./CynlinderCenter";
import ScreenBtnsBottom from "./ScreenBtnsBottom";
import ScreenBtnsTop from "./ScreenBtnsTop";
import ScreenRight from "./ScreenRight";
import PokemonName from "./PokemonName";
import PokedexButtons from "./PokedexButtons";
import PokedexTop from "./PokedexTop";
import axios from "axios";
import { colorType, bgArray } from "../assets/types";
import { EventBus } from "../plugins/event-bus";

export default {
  components: {
    ScreenRight,
    PokeAbilities,
    CynlinderCenter,
    PowerLeft,
    PowerRight,
    ScreenBtnsBottom,
    ScreenBtnsTop,
    PokemonImage,
    PokemonName,
    PokePowers,
    PokedexTop,
    PokedexButtons,
  },
  data() {
    return {
      pokedata: {
        number: "",
        type: "",
        height: "",
        weight: "",
        region: "",
        types: [],
      },
      stats: {},
      abilities: {},
      info: "",
      name: "",
      left: "<",
      right: ">",
      index: 1,
    };
  },
  created() {
    this.off = false;
    this.getRegion(this.index);
    EventBus.$on("powerOff", (data) => {
      this.off = data.off;
      data.off ? (this.info = data.info) : (this.info = "details");
    });
    EventBus.$on("getInfos", (data) => {
      data === "powers" ? this.getPowers(this.index) : "";
      data === "abilities" ? this.getAbilities(this.index) : "";
      data === "details"
        ? ((this.info = "details"),
          setTimeout(() => {
            this.setTypes();
          }, 100))
        : "";
    });
    this.info = "details";
  },
  methods: {
    async getPowers(index) {
      await axios
        .get(`https://pokeapi.co/api/v2/pokemon/${index}`)
        .then((res) => {
          this.stats = res.data.stats;
          this.info = "powers";
        });
    },
    async getAbilities(index) {
      await axios
        .get(`https://pokeapi.co/api/v2/pokemon/${index}`)
        .then((res) => {
          this.abilities = res.data.abilities;
          this.info = "abilities";
        });
    },
    formateIndex(index) {
      if (index < 10) {
        return `#00${index}`;
      } else if (index < 100) {
        return `#0${index}`;
      } else {
        return `#${index}`;
      }
    },
    getDetails($event) {
      setTimeout(() => {
        this.info = "details";
      }, 200);
      this.pokedata.height = $event.height;
      this.pokedata.weight = $event.weight;
      this.pokedata.number = this.formateIndex($event.id);
      this.pokedata.types = $event.types;
      this.name = $event.name;

      setTimeout(() => {
        this.setTypes();
      }, 200);

      let bg = bgArray;
      const bodyDOM = document.getElementsByTagName("body")[0];

      bodyDOM.style.backgroundImage = `url('${
        bg[Math.floor(Math.random() * bg.length)]
      }')`;
    },
    setTypes() {
      let colors = colorType;
      let types = [];

      this.pokedata.types.forEach((el) => {
        types.push(el.type.name);
      });

      const typesArray = types;

      this.pokedata.type = typesArray.join(" / ");

      const bubble1 = document.getElementById("bubble1");
      const bubble2 = document.getElementById("bubble2");

      if (this.pokedata.types.length === 2) {
        bubble1.style.backgroundColor =
          colors[this.pokedata.types[0].type.name];
        bubble2.style.backgroundColor =
          colors[this.pokedata.types[1].type.name];
      } else if (this.pokedata.types.length === 1) {
        bubble1.style.backgroundColor =
          colors[this.pokedata.types[0].type.name];
        bubble2.style.backgroundColor = "transparent";
      }
    },
    leftItem() {
      if (this.index > 1 && !this.off) {
        this.index--;
        this.getRegion(this.index);
      }
    },
    rightItem() {
      if (!this.off) {
        this.index++;
        this.getRegion(this.index);
      }
    },
    getRegion(i) {
      this.index = i;
      axios.get("https://pokeapi.co/api/v2/location/" + i).then((res) => {
        this.pokedata.region = res.data.region.name;
      });
    },
  },
};
</script>

<style>
</style>