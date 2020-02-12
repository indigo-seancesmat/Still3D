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
    },
    automatedPositions: {
      type: Array,
      default: null
    }
  },
  data() {
    return {
      app: null,
      x: 0,
      y: 0,
      width: window.innerWidth,
      height: window.innerHeight
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
    this.app = new PIXI.Application({
      width: this.width,
      height: this.height
    });
    this.$refs.still3d.appendChild(this.app.view);
    console.log(PIXI, this.app);
    let img = new PIXI.Texture(this.bgImage);
    let sprite = new PIXI.Sprite(img);
    img.bastTexture.on("loaded", () => {
      pauseVideo(video.baseTexture.source);
    });
    let containerRatio = this.width / this.height;
    let imageRatio = img.width / img.height;
    let scale = 0;
    let posX;
    let posY;
    if (containerRatio > imageRatio) {
      scale = this.width / img.width;
      posX = 0;
      posY = (this.height - img.height * scale) / 2;
    } else {
      scale = this.height / img.height;
      posX = (this.width - img.width * scale) / 2;
      posY = 0;
    }
    console.log(img.width, img.height, scale);
    img.scale.x = scale;
    img.scale.y = scale;
    img.position.x = posX;
    img.position.y = posY;
    this.app.stage.addChild(img);

    let depthMap = new PIXI.Sprite.from(this.depthMap);
    depthMap.scale.x = scale;
    depthMap.scale.y = scale;
    depthMap.position.x = posX;
    depthMap.position.y = posY;
    this.app.stage.addChild(depthMap);

    this.displacementFilter = new PIXI.filters.DisplacementFilter(depthMap);
    this.app.stage.filters = [this.displacementFilter];

    if (this.mouse === true) {
      let displacementFilter = new PIXI.filters.DisplacementFilter(depthMap);
      this.app.stage.filters = [displacementFilter];
      window.onmousemove = e => {
        displacementFilter.scale.x = (this.width / 2 - e.clientX) / 40;
        displacementFilter.scale.y = (this.height / 2 - e.clientY) / 40;
      };
      window.onclick = e => {
        console.log(
          "x: ",
          // (this.width / 2 - e.clientX) / 40,
          // (this.width / 2 - e.clientX) / 40 / this.width,
          e.clientX / this.width,
          "y: ",
          // (this.height / 2 - e.clientY) / 40,
          // (this.height / 2 - e.clientY) / 40 / this.height,
          // (this.height / 2 - this.height * 0.7) / 40,
          // this.height,
          e.clientY / this.height
        );
      };
    } else {
      this.displacementFilter.scale.x = 0;
      this.displacementFilter.scale.y = 0;
      this.tl = this.$anime.timeline({
        easing: "linear",
        duration: 1750,
        loop: true
      });
      this.tl.add({
        targets: this.$data,
        x: this.getXPos(0.53),
        y: this.getYPos(0.06)
      });
      this.tl.add({
        targets: this.$data,
        x: this.getXPos(0.7),
        y: this.getYPos(0.18)
      });
      this.tl.add({
        targets: this.$data,
        x: this.getXPos(0.94),
        y: this.getYPos(0.9)
      });
      this.tl.add({
        targets: this.$data,
        x: this.getXPos(0.34),
        y: this.getYPos(0.9)
      });
      this.tl.add({
        targets: this.$data,
        x: this.getXPos(0.32),
        y: this.getYPos(0.77)
      });
      this.tl.add({
        targets: this.$data,
        x: this.getXPos(0.07),
        y: this.getYPos(0.08)
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
      return (this.width / 2 - this.width * percent) / 40;
    },
    getYPos(percent) {
      // percentage between 0-1
      return (this.height / 2 - this.height * percent) / 40;
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
