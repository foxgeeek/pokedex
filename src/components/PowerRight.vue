<template>
  <div class="offScreen">
    <div id="off2" class="off">Desligando...</div>
  </div>
</template>

<script>
export default {
  methods: {
    ocultar() {
      let element = document.getElementById("off2");
      element.classList.add("hide2");
    },
  },
  created() {
    setTimeout(() => {
      this.ocultar();
    }, 200);
  },
};
</script>

<style>
.off {
  display: flex;
  align-items: center;
  justify-content: center;
  opacity: 1;
  transition: opacity 1.5s linear;
}

#off2.hide2 {
  opacity: 0;
  pointer-events: none;
}
</style>