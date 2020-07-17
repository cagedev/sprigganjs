<template>
  <div class="container">
    <canvas
      class="container"
      :width="this.scale * this.size_x"
      :height="this.scale * this.size_y"
      @mousedown.left="paint"
      :id="this.id"
    />
  </div>
</template>

<script>
export default {
  name: "PixelCanvas",
  props: {
    id: String,
    scale: Number,
    size_x: Number,
    size_y: Number,
    data_type: String,
    palette: {
      type: Object,
      default: function() {
        return {};
      },
    },
  },
  data() {
    return {
      data: [],
    };
  },
  methods: {
    showCoordinates(e) {
      console.log("x:" + e.offsetX + " y" + e.offsetY);
    },
    paint(e) {
      console.log("x:" + e.offsetX + " y" + e.offsetY);
      let cx = e.offsetX;
      let cy = e.offsetY;
      let rx = Math.floor(cx / this.scale);
      let ry = Math.floor(cy / this.scale);
      this.setPixel(rx, ry, "red");
    },
    setPixel(x, y, c) {
      var canvas = document.getElementById(this.id);
      console.log(c);
      var ctx = canvas.getContext("2d");
      console.log(ctx);
      ctx.beginPath();
      ctx.lineWidth = 0;
      ctx.fillStyle = c;
      ctx.rect(
        x * this.scale + 1,
        y * this.scale + 1,
        this.scale - 2,
        this.scale - 2
      );
      ctx.fill();
    },
  },
  mounted() {
    let range = (n) => Array.from(Array(n).keys());
    for (let i in range(this.size_x)) {
      for (let j in range(this.size_y)) {
        this.setPixel(i, j, "black");
      }
    }
  },
};
</script>

<style>
.container {
  border-style: solid;
  border-color: maroon;
  border-width: 1px;
}
</style>
