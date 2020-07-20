<template>
  <div class="container">
    <img :src="data" />
  </div>
</template>

<script>
export default {
  name: "Thumbnail",
  props: {
    data: String,
  },
};
</script>
