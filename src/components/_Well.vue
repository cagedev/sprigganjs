<template>
  <button
    :color="this.color"
    class="button"
    :style="cssVars"
    @mousedown.left="assignColor($event)"
    @mousedown.right="selectColor($event)"
    @contextmenu.capture.prevent
  />
</template>

<script>
export default {
  name: "Well",
  props: {
    color: String,
  },
  computed: {
    cssVars() {
      return {
        "background-color": this.color,
        color: this.color,
      };
    },
  },
  methods: {
    assignColor(e) {
      console.log(e);
    },
    selectColor(e) {
      console.log(e);
    },
  },
};
</script>

<style scoped>
.button {
  border: none;
  padding: 5px 10px;
  margin: 5px;
  height: 50px;
  width: 50px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  transition-duration: 0.5s;
}
.button:hover {
  background-color: var(background-color);
}
</style>
