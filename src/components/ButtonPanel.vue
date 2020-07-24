<template>
  <div class="container">
    <button @click="click_debug">Debug</button>
    <input type="file" id="input" @change="click_loadImage" />
    <button @click="click_saveImage">Save</button>
    <button @click="click_saveImageThumb">SaveThumb</button>
  </div>
</template>

<script>
export default {
  name: "Buttons",
  props: {},
  methods: {
    click_debug() {
      this.$emit('click-debug');
    },
    click_loadImage() {},
    click_saveImage() {},
    click_saveImageThumb() {},
  },
  mounted() {},
};
</script>
