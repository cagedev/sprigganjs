<template>
  <div class="container">
    <div>
      fgColor:<br>
      <v-swatches
        @input="$emit('color-change', 'fgColor', $event)"
        v-model="colors.fgColor"
        v-bind:swatches="palette_colors"
        show-labels show-border show-fallback
      />
    </div>
    <div>
      bgColor:<br>
      <v-swatches
        @input="$emit('color-change', 'bgColor', $event)"
        v-model="colors.bgColor"
        v-bind:swatches="palette_colors"
        show-labels show-border show-fallback
      ></v-swatches>
    </div>
  </div>
</template>

<script>
import VSwatches from "vue-swatches";

import "vue-swatches/dist/vue-swatches.css";

export default {
  name: "Palette",
  components: {
    VSwatches,
  },
  props: {
    id: String,
    colors: Object,
    palette_colors: Array,
  },
};
</script>
