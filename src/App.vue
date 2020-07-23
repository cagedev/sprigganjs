<template>
  <div id="app">
    <PixiePanel />
  </div>
</template>

<script>
import PixiePanel from "./components/PixiePanel.vue";

export default {
  name: "App",
  components: {
    PixiePanel,
  },
  methods: {},
  data() {
    return {
      width: 32,
      height: 32,
      image_data: new ImageData(32, 32),
      // {
      //   width: 32,
      //   height: 32,
      //   data: new Uint8ClampedArray(this.width * this.height * 4),
      // },
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
  /* color: #2c3e50; */
  margin-top: 60px;
  display: flex;
  background-color: yellow;
}

/* debug */
.container {
  border-style: solid;
  border-width: 1px;
}
.invisible {
  visibility: hidden;
  display: none;
}
</style>
