<template>
  <div class="container">
    AutoImage<br />
    <img :src="dataURL" />
  </div>
</template>

<script>
export default {
  name: "AutoImage",
  props: {
    dataURL: { type: String, required: true },
  },
};
</script>
