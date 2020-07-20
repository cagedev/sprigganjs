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
  },
  data() {
    return {
      data: [],
    };
  },
  methods: {
    paint(e, c) {
      let cx = e.offsetX;
      let cy = e.offsetY;
      let rx = Math.floor(cx / (this.scale + this.spacing));
      let ry = Math.floor(cy / (this.scale + this.spacing));
      // this.setPixel(rx, ry, c);
      console.log("setPixel("+rx+","+ry+","+c+")");
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
