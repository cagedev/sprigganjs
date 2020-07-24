<template>
  <div class="container">
    ClickLayer <br />
    <canvas
      class="container"
      :width="this.scale * this.size_x"
      :height="this.scale * this.size_y"
      @contextmenu.capture.prevent
      @mousedown.left="clickPixel($event, 1)"
      @mousedown.right="clickPixel($event, 0)"
    />
  </div>
</template>

<script>
export default {
  name: "ClickLayer",
  props: {
    scale: { type: Number, default: 10 },
    size_x: { type: Number, required: true },
    size_y: { type: Number, required: true },
  },
  methods: {
    clickPixel(e, mb) {
      let cx = e.offsetX;
      let cy = e.offsetY;
      let rx = Math.floor(cx / this.scale);
      let ry = Math.floor(cy / this.scale);
      // this.setPixel(rx, ry, c);
      // console.log("setPixel(" + rx + "," + ry + "," + c + ")");
      this.$emit('clicked-pixel', rx, ry, mb);
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
