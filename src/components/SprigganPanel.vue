<template>
  <div class="container">
    <AutoImage :dataURL="dataURL" />
    <AutoCanvas :width="width" :height="height" :pixels="pixels" />
    <div class="container">
      <PixelCanvas
        class="stack"
        :scale="10"
        :size_x="width"
        :size_y="height"
        :pixels="pixels"
      />
      <ClickLayer
        class="stack"
        :scale="10"
        :size_x="width"
        :size_y="height"
        @clicked-pixel="handleClickedPixel"
      />
    </div>
    <ButtonPanel @click-debug="debug" />
    <PalettePanel
      :colors="pixelcanvas_colors"
      :palette_colors="palette_colors"
      @color-change="handleColorChange"
    />
  </div>
</template>

<script>
import AutoImage from "./AutoImage.vue";
import AutoCanvas from "./AutoCanvas.vue";
import PixelCanvas from "./PixelCanvas.vue";
import ClickLayer from "./ClickLayer.vue";
import ButtonPanel from "./ButtonPanel.vue";
import PalettePanel from "./PalettePanel.vue";

export default {
  name: "SprigganPanel",
  components: {
    AutoImage,
    AutoCanvas,
    PixelCanvas,
    ClickLayer,
    ButtonPanel,
    PalettePanel,
  },
  props: {
    width: { type: Number, required: true },
    height: { type: Number, required: true },
  },
  data() {
    return {
      pixels: [],
      dbg_index: 0,
      fgColor: [0, 0, 255, 255],
      bgColor: [255, 128, 0, 255],
      // fgColor: "rgb(0, 0, 255)",
      // bgColor: "#FF8000",
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
      // pixelcanvas_colors: {
      //   fgColor: "#CC0001",
      //   bgColor: "#000000",
      // },
    };
  },
  computed: {
    pixelcanvas_colors: function () {
      return {
        'fgColor': this.getColorString(this.fgColor), 
        'bgColor': this.getColorString(this.bgColor),
      };
    },
    dataURL: function () {
      if (this.pixels.length !== 0) {
        let osc = document.createElement("canvas");
        osc.width = this.width;
        osc.height = this.height;
        let ctx = osc.getContext("2d");
        let img_data = new ImageData(
          new Uint8ClampedArray(this.pixels),
          this.width,
          this.height
        );
        // console.log(img_data);
        ctx.putImageData(img_data, 0, 0);
        return osc.toDataURL("image/png");
      } else {
        return "";
      }
    },
  },
  mounted() {
    // initialize pixel array
    for (let j = 0; j < this.height; j++) {
      for (let i = 0; i < this.width; i++) {
        this.setpixel(i, j, 128, 128, 128, 255);
      }
    }
  },
  methods: {
    // use reactive array assignment
    setpixel: function (x, y, r, g, b, a) {
      this.$set(this.pixels, (this.width * y + x) * 4 + 0, r);
      this.$set(this.pixels, (this.width * y + x) * 4 + 1, g);
      this.$set(this.pixels, (this.width * y + x) * 4 + 2, b);
      this.$set(this.pixels, (this.width * y + x) * 4 + 3, a);
    },
    handleClickedPixel: function (x, y, mb) {
      let colors = [
        this.palette_colors["bgColor"],
        this.palette_colors["fgColor"],
      ];
      this.setpixel(x, y, ...colors[mb]);
      // console.log("x:", x, " y:", y, " c:", ...colors[mb]);
    },
    handleColorChange: function (color_name, color_string) {
      console.log(color_name, color_string);
      // this.pixelcanvas_colors[color_name] = this.getColorArray(color_string);
      this.color_name = this.getColorArray(color_string);
      // this.c = e;
    },
    getColorArray: function (color_string) {
      let r = parseInt(color_string.substring(1, 2), 16);
      let g = parseInt(color_string.substring(3, 4), 16);
      let b = parseInt(color_string.substring(5, 6), 16);
      return [r, b, g, 255];
    },
    getColorString: function (color_array) {
      let cs = "#";
      cs = cs + color_array[0].toString(16).padStart(2, 0);
      cs = cs + color_array[1].toString(16).padStart(2, 0);
      cs = cs + color_array[2].toString(16).padStart(2, 0);
      return cs;
    },
    // make the pixel at `index` yellow
    debug: function () {
      this.setpixel(
        this.dbg_index % this.width,
        Math.floor(this.dbg_index / this.width),
        255,
        255,
        0,
        255
      );
      this.dbg_index = (this.dbg_index + 1) % (this.width * this.height);
    },
  },
};
</script>

<style>
.stack {
  position: absolute;
  left: 0px;
  top: 200px;
}
</style>
