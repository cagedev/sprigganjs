<template>
  <div class="container">
    <!-- Thumbnail:
    <Thumbnail :data="image_dataurl" /> -->
    PixelCanvas:<br />
    <!-- <PixelCanvas
      :scale="20"
      :spacing="1"
      :size_x="32"
      :size_y="32"
      id="myCanvas"
      palette_id="myPalette"
      v-bind.sync="pixelcanvas_colors"
      v-bind:imgData.sync="image_data"
      v-on:draw-pixel="setPixel"
    /> -->
    <TestCanvas
      :width="100"
      :height="100"
      :draw="
        (canvas, ctx) => {
          ctx.putImageData(new ImageData(this.image_data, this.size_x), 0, 0);
        }
      "
    />
    Palette: <br />
    <Palette
      id="myPalette"
      v-bind:colors="pixelcanvas_colors"
      @color-change="changeColor"
      v-bind:palette_colors="palette_colors"
    />
    Buttons:<br />
    <Buttons />
  </div>
</template>

<script>
// import PixelCanvas from "./PixelCanvas.vue";
import TestCanvas from "./TestCanvas.vue";
import Palette from "./Palette.vue";
import Buttons from "./Buttons.vue";
// import Thumbnail from "./Thumbnail.vue";

import { saveAs } from "file-saver";

export default {
  name: "PixiePanel",
  components: {
    // PixelCanvas,
    TestCanvas,
    Palette,
    Buttons,
    // Thumbnail,
  },
  props: {
    // image_data: ImageData,
    // type: Object,
    // {
    //   type: Array,
    // //   default: () => [0, 0, 0, 0],
    //   default: this.getEmptyArray(),
    // },
    image_mode: {
      type: String,
      default: "RGBA",
    },
    size_x: {
      type: Number,
      default: 32,
    },
    size_y: {
      type: Number,
      default: 32,
    },
    x: {
      type: Number,
      default: 10,
    },
  },
  computed: {
    image_data: {
      get() {
        let c = document.createElement("canvas");
        let ctx = c.getContext("2d");
        ctx.canvas.width = this.size_x;
        ctx.canvas.height = this.size_y;
        ctx.rect(1, 1, 1, 1);
        ctx.fill();
        return ctx.getImageData(0, 0, c.width, c.height).data;
      },
      set() {
        var ctx = this.$refs.canvas.getContext("2d");
        ctx.putImageData(this.image_data, 0, 0);
      },
    },
    // image_dataurl: function () {
    //   if (this.image_data) {
    //     let c = document.createElement("canvas");
    //     let ctx = c.getContext("2d");
    //     ctx.canvas.width = this.size_x;
    //     ctx.canvas.height = this.size_y;
    //     ctx.putImageData(new ImageData(this.image_data, this.size_x), 0, 0);
    //     return c.toDataURL("image/png");
    //   } else {
    //     return "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACAAAAAgCAYAAABzenr0AAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAKBJREFUeNpiYBjpgBFd4P///wJAaj0QO9DEQiAg5ID9tLIcmwMYsDgABhqoaTHMUHRxpsGYBv5TGqTIZsDkYWLo6gc8BEYdMOqAUQeMOoAqDgAWcgZAfB9EU63SIAGALH8PZb+H8v+jVz64KiOK6wIg+ADEArj4hOoCajiAqMpqtDIadcCoA0YdQIoDDtCqQ4KtBY3NAYG0csQowAYAAgwAgSqbls5coPEAAAAASUVORK5CYII=";
    //   }
    // },
  },
  // watch: {
  //   image_data: {
  //     function() {
  //       console.log("imagedata_cahnged");
  //     },
  //     deep: true,
  //   },
  // },
  methods: {
    getEmptyArray() {
      return new Array(this.size_x * this.size_y * 4).fill(128);
    },
    changeColor(brush, color) {
      this.pixelcanvas_colors[brush] = color;
      this.image_data.data[0] = 255;
      this.image_data.data[1] = 0;
      this.image_data.data[2] = 0;
      this.image_data.data[3] = 255;
      //   console.log(this.image_data);
      this.image_data.width = this.image_data.width + 1;
    },
    saveImage() {
      var canvas = document.getElementById(this.id);
      canvas.toBlob(function (blob) {
        saveAs(blob);
      });
      // window.location.replace(canvas.toDataURL('image/png'));
    },
    saveImageThumb() {},
    loadImage() {
      let f = event.target.files;
      // console.log(f);
      if (f.length > 0) {
        let fr = new FileReader();
        fr.onload = () => {
          let img = document.getElementById("myImgPlaceholder");
          let img2 = document.getElementById("myImgPlaceholder2");
          img.src = fr.result;
          img.onload = () => {
            // let c = document.createElement("canvas");
            let c = document.getElementById("myCanvasPlaceholder");
            c.width = this.size_x;
            c.height = this.size_y;
            let ctx = c.getContext("2d");
            ctx.imageSmoothingEnabled = false;
            ctx.drawImage(img, 0, 0, this.size_x, this.size_y);
            // debug: show resized version in img
            img2.src = c.toDataURL();
            // debug: log data
            let imgData = ctx.getImageData(0, 0, this.size_x, this.size_x).data;
            console.log(imgData);

            // -> this.paintImage()
            // paint to interface (slow way to make it pretty)
            for (let j = 0; j < this.size_y; j++) {
              for (let i = 0; i < this.size_x; i++) {
                let pi = (j * this.size_x + i) * 4;
                let cs = "#";
                cs = cs + imgData[pi + 0].toString(16).padStart(2, 0);
                cs = cs + imgData[pi + 1].toString(16).padStart(2, 0);
                cs = cs + imgData[pi + 2].toString(16).padStart(2, 0);
                let mask =
                  "#" +
                  imgData[pi + 3].toString(16).padStart(2, 0) +
                  imgData[pi + 3].toString(16).padStart(2, 0) +
                  imgData[pi + 3].toString(16).padStart(2, 0);
                if (this.load_mask) {
                  this.setPixel(i, j, mask);
                } else {
                  if (imgData[pi + 3] == 255) {
                    this.setPixel(i, j, cs);
                  } else {
                    this.setPixel(i, j, this.bgColor);
                  }
                }
              }
            }
          };
        };
        fr.readAsDataURL(f[0]);
      }
    },
    paint(e, c) {
      let cx = e.offsetX;
      let cy = e.offsetY;
      let rx = Math.floor(cx / this.scale);
      let ry = Math.floor(cy / this.scale);
      this.setPixel(rx, ry, c);
    },
    setPixel(x, y, r, g, b) {
      this.image_data.data[(x + y * this.image_data.width) * 4 + 0] = r;
      this.image_data.data[(x + y * this.image_data.width) * 4 + 1] = g;
      this.image_data.data[(x + y * this.image_data.width) * 4 + 2] = b;
    },
    setFgColor(c) {
      this.fgColor = c;
    },
  },
  data() {
    return {
      image_data: null,
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
      local_image_data: this.image_data,
      local_image_dataurl: this.image_dataurl,
    };
  },
};
</script>
