<template>
  <div class="container">
    <!-- image loading -->
    <input type="file" id="input" @change="click_loadImage" />
    <!-- images saving -->
    <div class="container">
      <button @click="click_saveImage">Save</button>
      <button @click="click_saveImageThumb">SaveThumb</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "Buttons",
  props: {},
  methods: {
    click_loadImage() {},
    click_saveImage() {},
    click_saveImageThumb() {},
  },
  mounted() {},
};
</script>
