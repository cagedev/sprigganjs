<template>
  <div class="container">
    PixelCanvas <br />
    <canvas
      class="container"
      :width="this.scale * this.size_x"
      :height="this.scale * this.size_y"
      @contextmenu.capture.prevent
      ref="pixelcanvas"
    />
    <!-- @input="setFgColor($event.payload[0])" -->
  </div>
</template>

<!-- @mousedown.left="paint($event, fgColor)"
      @mousedown.right="paint($event, bgColor)" -->

<script>
export default {
  name: "PixelCanvas",
  props: {
    scale: { type: Number, default: 10 },
    spacing: { type: Number, default: 1 },
    size_x: { type: Number, required: true },
    size_y: { type: Number, required: true },
    fgColor: {
      type: String,
      default: "red",
    },
    bgColor: {
      type: String,
      default: "black",
    },
    load_mask: Boolean,
    pixels: { type: Array, required: true },
  },
  watch: {
    pixels: {
      handler: function() {
        console.log(this.pixels.length);
        this.drawData();
      },
    },
  },
  methods: {
    // paint(e, c) {
    //   let cx = e.offsetX;
    //   let cy = e.offsetY;
    //   let rx = Math.floor(cx / this.scale);
    //   let ry = Math.floor(cy / this.scale);
    //   // this.setPixel(rx, ry, c);
    //   // console.log("setPixel(" + rx + "," + ry + "," + c + ")");
    //   this.drawPixel(rx, ry, c);
    // },
    drawData() {
      console.log("drawData() for " + this.pixels);
      for (let j = 0; j < this.size_y; j++) {
        for (let i = 0; i < this.size_x; i++) {
          let pindex = (j * this.size_x + i) * 4;
          // let cs = "#";
          // cs = cs + this.imgData.data[pindex + 0].toString(16).padStart(2, 0);
          // cs = cs + this.imgData.data[pindex + 1].toString(16).padStart(2, 0);
          // cs = cs + this.imgData.data[pindex + 2].toString(16).padStart(2, 0);
          // let mask =
          //   "#" +
          //   this.imgData.data[pi + 3].toString(16).padStart(2, 0) +
          //   this.imgData.data[pi + 3].toString(16).padStart(2, 0) +
          //   this.imgData.data[pi + 3].toString(16).padStart(2, 0);
          // if (this.load_mask) {
          //   this.setPixel(i, j, mask);
          // } else {
          if (this.pixels[pindex + 3] == 255) {
            let r = this.pixels[pindex + 0];
            let g = this.pixels[pindex + 1];
            let b = this.pixels[pindex + 2];
            this.drawPixel(i, j, `rgb(` + r + `, ` + g + `, ` + b + `)`);
          } else {
            this.drawPixel(i, j, this.bgColor);
          }
        }
      }
    },
    drawPixel(x, y, c) {
      // let r = parseInt(c.substring(1, 2), 16);
      // let g = parseInt(c.substring(3, 4), 16);
      // let b = parseInt(c.substring(5, 6), 16);
      // this.$emit("draw-pixel", x, y, r, g, b);
      // var canvas = document.getElementById(this.id);
      let canvas = this.$refs["pixelcanvas"];
      var ctx = canvas.getContext("2d");
      ctx.beginPath();
      ctx.lineWidth = 0;
      // ctx.fillStyle = rgb(r, g, b);
      ctx.fillStyle = c;
      ctx.rect(
        x * this.scale,
        y * this.scale,
        this.scale - this.spacing,
        this.scale - this.spacing
      );
      ctx.fill();
    },
  },
  // mounted() {
  //   let range = (n) => Array.from(Array(n).keys());
  //   for (let i in range(this.size_x)) {
  //     for (let j in range(this.size_y)) {
  //       this.setPixel(i, j, this.bgColor);
  //     }
  //   }
  // },
};
</script>
