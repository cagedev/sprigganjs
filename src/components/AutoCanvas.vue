<template>
  <div class="container">
    <canvas :width="width" :height="height" ref="autocanvas" />
  </div>
</template>

<script>
export default {
  name: "AutoCanvas",
  props: {
    pixels: Array,
    width: Number,
    height: Number,
  },
  watch: {
    pixels: {
      handler: function() {
        this.render();
      },
    },
  },
  methods: {
    render() {
      let ctx = this.$refs['autocanvas'].getContext("2d");
      let img_data = new ImageData(
        new Uint8ClampedArray(this.pixels),
        this.width,
        this.height
      );
      ctx.putImageData(img_data, 0, 0);
    },
  },
};
</script>
