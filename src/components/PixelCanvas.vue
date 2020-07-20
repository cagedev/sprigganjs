<template>
  <div class="container">
    <div class="container">
      <canvas
        class="container"
        :width="this.scale * this.size_x"
        :height="this.scale * this.size_y"
        @mousedown.left="paint($event, fgColor)"
        @mousedown.right="paint($event, bgColor)"
        @contextmenu.capture.prevent
        :id="this.id"
        @input="setFgColor($event.payload[0])"
      />
    </div>
    <!-- image loading -->
    <div class="container">
      <input type="file" id="input" multiple @change="loadImage" />
      <div class="invisible">
        <!-- holds loaded image -->
        myImgPlaceholder<br />
        <img id="myImgPlaceholder" class="container invisible" /><br />
        myImgPlaceholder2<br />
        <img id="myImgPlaceholder2" class="container invisible" /><br />
        <!-- holds manipulated image -->
        myCanvasPlaceholder<br />
        <canvas id="myCanvasPlaceholder" class="container invisible" /><br />
      </div>
    </div>
    <!-- images saving -->
    <div class="container">
      <button @click="saveImage">Save</button>
      <button @click="saveImageThumb">SaveThumb</button>
    </div>
  </div>
</template>

<script>
import { saveAs } from "file-saver";

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
    saveImage() {
      var canvas = document.getElementById(this.id);
      canvas.toBlob(function (blob) {
        saveAs(blob);
      });
      // window.location.replace(canvas.toDataURL('image/png'));
    },
    saveImageThumb() {},
    loadImage() {
      let f = event.target.files;
      // console.log(f);
      if (f.length > 0) {
        let fr = new FileReader();
        fr.onload = () => {
          let img = document.getElementById("myImgPlaceholder");
          let img2 = document.getElementById("myImgPlaceholder2");
          img.src = fr.result;
          img.onload = () => {
            // let c = document.createElement("canvas");
            let c = document.getElementById("myCanvasPlaceholder");
            c.width = this.size_x;
            c.height = this.size_y;
            let ctx = c.getContext("2d");
            ctx.imageSmoothingEnabled = false;
            ctx.drawImage(img, 0, 0, this.size_x, this.size_y);
            // debug: show resized version in img
            img2.src = c.toDataURL();
            // debug: log data
            let imgData = ctx.getImageData(0, 0, this.size_x, this.size_x).data;
            console.log(imgData);

            // -> this.paintImage()
            // paint to interface (slow way to make it pretty)
            for (let j = 0; j < this.size_y; j++) {
              for (let i = 0; i < this.size_x; i++) {
                let pi = (j * this.size_x + i) * 4;
                let cs = "#";
                cs = cs + imgData[pi + 0].toString(16).padStart(2, 0);
                cs = cs + imgData[pi + 1].toString(16).padStart(2, 0);
                cs = cs + imgData[pi + 2].toString(16).padStart(2, 0);
                let mask =
                  "#" +
                  imgData[pi + 3].toString(16).padStart(2, 0) +
                  imgData[pi + 3].toString(16).padStart(2, 0) +
                  imgData[pi + 3].toString(16).padStart(2, 0);
                if (this.load_mask) {
                  this.setPixel(i, j, mask);
                } else {
                  if (imgData[pi + 3] == 255) {
                    this.setPixel(i, j, cs);
                  } else {
                    this.setPixel(i, j, this.bgColor);
                  }
                }
              }
            }
          };
        };
        fr.readAsDataURL(f[0]);
      }
    },
    paint(e, c) {
      let cx = e.offsetX;
      let cy = e.offsetY;
      let rx = Math.floor(cx / this.scale);
      let ry = Math.floor(cy / this.scale);
      this.setPixel(rx, ry, c);
    },
    setPixel(x, y, c) {
      var canvas = document.getElementById(this.id);
      var ctx = canvas.getContext("2d");
      ctx.beginPath();
      ctx.lineWidth = 0;
      ctx.fillStyle = c;
      ctx.rect(
        x * this.scale,
        y * this.scale,
        this.scale - this.spacing,
        this.scale - this.spacing
      );
      ctx.fill();
    },
    setFgColor(c) {
      this.fgColor = c;
    },
  },
  mounted() {
    let range = (n) => Array.from(Array(n).keys());
    for (let i in range(this.size_x)) {
      for (let j in range(this.size_y)) {
        this.setPixel(i, j, this.bgColor);
      }
    }
  },
};
</script>

<style>
.container {
  border-style: none;
}
.invisible {
  visibility: hidden;
  display: none;
}
</style>
