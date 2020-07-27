<template>
  <div class="screen-black">
    <!-- <img :src="image" alt id="image" /> -->
    <div class="flip-container">
      <div class="flipper">
        <div class="front">
          <img :src="front" alt class="image" />
        </div>
        <div class="back">
          <img :src="back" alt class="image" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  props: {
    index: { type: Number, required: true },
  },
  data() {
    return {
      front: "",
      back: "",
    };
  },
  created() {
    this.fetchPokemon(this.index);
  },
  watch: {
    index: {
      async handler() {
        await document.getElementById("blueBtn").classList.add("active");
        await setTimeout(() => {
          document.getElementById("blueBtn").classList.remove("active");
        }, 500);
        return await this.fetchPokemon(this.index);
      },
    },
  },
  methods: {
    async fetchPokemon(index) {
      await axios
        .get(`https://pokeapi.co/api/v2/pokemon/${index}`)
        .then((res) => {
          this.$emit("pokemonDetails", res.data);
          this.front = res.data.sprites.front_default;
          this.back = res.data.sprites.back_default;
        });
    },
  },
};
</script>

<style>
.screen-black {
  -ms-grid-row: 2;
  -ms-grid-row-span: 1;
  grid-row: 2/3;
  background-color: var(--blackScreen);
  border-radius: 3px;
  width: 100%;
  height: 100%;
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-pack: center;
  -ms-flex-pack: center;
  justify-content: center;
  align-items: center;
}

.image {
  height: 100%;
  margin: 0 auto;
}

/* define a velocidade da transição */
.flipper {
  transition: 0.6s;
  -webkit-animation: spin 3000ms linear infinite;
  animation: spin 3000ms linear infinite;
  transform-style: preserve-3d;
}

@-webkit-keyframes spin {
  100% {
    -webkit-transform: rotateY(360deg);
  }
}

@keyframes spin {
  100% {
    transform: rotateY(360deg);
  }
}

/* esconde o verso durante a animação */
.front,
.back {
  backface-visibility: hidden;
  position: relative;
  top: 0;
  left: 0;
}

/* frente posicionada sobre o verso */
.front {
  z-index: 2;
}

/* verso inicialmente escondido */
.back {
  position: absolute;
  transform: rotateY(180deg);
}
</style>