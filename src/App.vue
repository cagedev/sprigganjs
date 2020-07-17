<template>
  <div id="app">
    <PixelCanvas :scale="10" :size_x="32" :size_y="32" id="myCanvas" />
    <Palette :num_colors="10" />
  </div>
</template>

<script>
import PixelCanvas from "./components/PixelCanvas.vue";
import Palette from "./components/Palette.vue";

export default {
  name: "App",
  components: {
    PixelCanvas,
    Palette,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
