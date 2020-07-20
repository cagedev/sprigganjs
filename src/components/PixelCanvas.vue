<template>
  <div class="container">
    <canvas
      class="container"
      :width="this.scale * this.size_x"
      :height="this.scale * this.size_y"
      @mousedown.left="paint($event, fgColor)"
      @mousedown.right="paint($event, bgColor)"
      @contextmenu.capture.prevent
      :id="this.id"
    />
    <!-- @input="setFgColor($event.payload[0])" -->
  </div>
</template>

<script>
export default {
  name: "PixelCanvas",
  props: {
    id: {
      type: String,
      required: true,
    },
    scale: Number,
    spacing: Number,
    size_x: Number,
    size_y: Number,
    fgColor: {
      type: String,
      default: "red",
    },
    bgColor: {
      type: String,
      default: "black",
    },
    load_mask: Boolean,
    imgData: ImageData,
  },
  watched: {
    imgData: () =>   {
      this.drawData();
    },
  },
  methods: {
    paint(e, c) {
      let cx = e.offsetX;
      let cy = e.offsetY;
      let rx = Math.floor(cx / (this.scale + this.spacing));
      let ry = Math.floor(cy / (this.scale + this.spacing));
      // this.setPixel(rx, ry, c);
      console.log("setPixel(" + rx + "," + ry + "," + c + ")");
    },
    drawData() {
      console.log('drawData() for ' + this.imgData);
      for (let j = 0; j < this.imgData.height; j++) {
        for (let i = 0; i < this.imgData.width; i++) {
          let pi = (j * this.imgData.width + i) * 4;
          let cs = "#";
          cs = cs + this.imgData.data[pi + 0].toString(16).padStart(2, 0);
          cs = cs + this.imgData.data[pi + 1].toString(16).padStart(2, 0);
          cs = cs + this.imgData.data[pi + 2].toString(16).padStart(2, 0);
          let mask =
            "#" +
            this.imgData.data[pi + 3].toString(16).padStart(2, 0) +
            this.imgData.data[pi + 3].toString(16).padStart(2, 0) +
            this.imgData.data[pi + 3].toString(16).padStart(2, 0);
          if (this.load_mask) {
            this.setPixel(i, j, mask);
          } else {
            if (this.imgData.data[pi + 3] == 255) {
              this.drawPixel(i, j, cs);
            } else {
              this.drawPixel(i, j, this.bgColor);
            }
          }
        }
      }
    },
    drawPixel(x, y, c) {
      var canvas = document.getElementById(this.id);
      var ctx = canvas.getContext("2d");
      ctx.beginPath();
      ctx.lineWidth = 0;
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
