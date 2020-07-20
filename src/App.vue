<template>
  <div id="app">
    <PixelCanvas
      :scale="20"
      :spacing="1"
      :size_x="32"
      :size_y="32"
      id="myCanvas"
      palette_id="myPalette"
      v-bind.sync="pixelcanvas_colors"
    />
    <Palette
      id="myPalette"
      v-bind:colors="pixelcanvas_colors"
      @color-change="changeColor"
      v-bind:palette_colors="palette_colors"
    />
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
  methods: {
    changeColor(brush, color) {
      this.pixelcanvas_colors[brush] = color;
    },
  },
  data() {
    return {
      palette_colors: [
        "#CC0001",
        "#E36101",
        "#FFCC00",
        "#009900",
        "#0066CB",
        "#000000",
        "#FFFFFF",
        "#ff00ff",
      ],
      pixelcanvas_colors: {
        fgColor: "#CC0001",
        bgColor: "#000000",
      },
    };
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
  display: flex;
}
</style>
