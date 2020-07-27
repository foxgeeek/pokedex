<template>
  <div class="screen-black">
    <div id="off" class="off">Desligando...</div>
  </div>
</template>

<script>
export default {
  methods: {
    ocultar() {
      let element = document.getElementById("off");
      element.classList.add("hide");
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

#off.hide {
  opacity: 0;
  pointer-events: none;
}
</style>