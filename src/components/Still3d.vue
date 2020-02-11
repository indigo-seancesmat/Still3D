<template>
  <div ref="still3d" class="still3d"></div>
</template>

<script>
import * as PIXI from "pixi.js";

export default {
  name: "Still3d",
  props: {
    depthMap: {
      type: String,
      required: true
    },
    bgImage: {
      type: String,
      required: true
    },
    mouse: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      app: null,
      x: 0,
      y: 0
    };
  },
  watch: {
    x(newValue) {
      this.displacementFilter.scale.x = newValue;
    },
    y(newValue) {
      this.displacementFilter.scale.y = newValue;
    }
  },
  mounted() {
    this.width = window.innerWidth;
    this.height = window.innerHeight;
    this.app = new PIXI.Application({
      width: window.innerWidth,
      height: window.innerHeight
    });
    this.$refs.still3d.appendChild(this.app.view);

    let img = new PIXI.Sprite.from(this.bgImage);
    img.width = window.innerWidth;
    img.height = window.innerHeight;
    this.app.stage.addChild(img);

    let depthMap = new PIXI.Sprite.from(this.depthMap);
    depthMap.width = window.innerWidth;
    depthMap.height = window.innerHeight;
    this.app.stage.addChild(depthMap);

    this.displacementFilter = new PIXI.filters.DisplacementFilter(depthMap);
    this.app.stage.filters = [this.displacementFilter];

    if (this.mouse === true) {
      let displacementFilter = new PIXI.filters.DisplacementFilter(depthMap);
      this.app.stage.filters = [displacementFilter];
      window.onmousemove = function(e) {
        displacementFilter.scale.x = (window.innerWidth / 2 - e.clientX) / 40;
        displacementFilter.scale.y = (window.innerHeight / 2 - e.clientY) / 40;
      };
      window.onclick = function(e) {
        console.log(
          "x: ",
          // (window.innerWidth / 2 - e.clientX) / 40,
          // (window.innerWidth / 2 - e.clientX) / 40 / window.innerWidth,
          e.clientX / window.innerWidth,
          "y: ",
          // (window.innerHeight / 2 - e.clientY) / 40,
          // (window.innerHeight / 2 - e.clientY) / 40 / window.innerHeight,
          // (window.innerHeight / 2 - window.innerHeight * 0.7) / 40,
          // window.innerHeight,
          e.clientY / window.innerHeight
        );
      };
    } else {
      this.displacementFilter.scale.x = 0;
      this.displacementFilter.scale.y = 0;
      this.tl = this.$anime.timeline({
        easing: "linear",
        duration: 1000,
        loop: true
      });
      this.tl.add({
        targets: this.$data,
        x: window.innerWidth * -0.007,
        y: window.innerHeight * 0.011
      });
      this.tl.add({
        targets: this.$data,
        x: window.innerWidth * -0.006,
        y: window.innerHeight * 0.005
      });
      this.tl.add({
        targets: this.$data,
        x: window.innerWidth * 0.012,
        y: window.innerHeight * -0.012
      });
      this.tl.add({
        targets: this.$data,
        x: window.innerWidth * 0.009,
        y: window.innerHeight * 0.012
      });
      this.tl.add({
        targets: this.$data,
        x: 0,
        y: 0
      });
    }
  },
  methods: {
    getXPos(percent) {
      // percentage between 0-1
      return (window.innerWidth / 2 - window.innerWidth * percent) / 40;
    },
    getYPos(percent) {
      // percentage between 0-1
      return (window.innerHeight / 2 - window.innerHeight * percent) / 40;
    }
  }
};
</script>

<style lang="scss" scoped>
.still3d {
  h1 {
    color: blue;
  }
}
</style>
