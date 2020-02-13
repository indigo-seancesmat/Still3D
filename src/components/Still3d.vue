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
    mouseSensitivity: {
      type: Number,
      default: 40
    },
    animationSpeed: {
      type: Number,
      default: 1000
    },
    automatedPositions: {
      type: Array,
      default: () => {
        return [
          {
            x: 0.53,
            y: 0.06
          },
          {
            x: 0.7,
            y: 0.18
          },
          {
            x: 0.94,
            y: 0.9
          },
          {
            x: 0.34,
            y: 0.9
          },
          {
            x: 0.32,
            y: 0.77
          },
          {
            x: 0.07,
            y: 0.08
          },
          {
            x: 0,
            y: 0
          }
        ];
      }
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
  beforeDestroy() {
    if (this.ticker) {
      this.ticker.stop();
    }
  },
  mounted() {
    this.app = new PIXI.Application({
      width: this.width,
      height: this.height,
      resolution: window.devicePixelRatio,
      autoDensity: true
    });
    this.$refs.still3d.appendChild(this.app.view);

    this.imgTexture = PIXI.Texture.from(this.bgImage);
    this.img = new PIXI.Sprite(this.imgTexture);
    this.imgTexture.baseTexture.on("loaded", () => {
      this.setScale();
    });
    this.img.anchor.x = 0.5;
    this.img.anchor.y = 0.5;
    this.app.stage.addChild(this.img);

    this.depthMapCont = new PIXI.Sprite.from(this.depthMap);
    this.depthMapCont.anchor.x = 0.5;
    this.depthMapCont.anchor.y = 0.5;
    this.app.stage.addChild(this.depthMapCont);

    this.setScale();

    this.displacementFilter = new PIXI.filters.DisplacementFilter(
      this.depthMapCont
    );
    this.app.stage.filters = [this.displacementFilter];

    if (this.mouse === true) {
      window.onmousemove = e => {
        this.displacementFilter.scale.x = (this.width / 2 - e.clientX) / 40;
        this.displacementFilter.scale.y = (this.height / 2 - e.clientY) / 40;
      };
      window.onclick = e => {
        console.log(
          `x: ${e.clientX / this.width} y: ${e.clientY / this.height}`
        );
      };
    } else {
      this.displacementFilter.scale.x = 0;
      this.displacementFilter.scale.y = 0;
      this.tl = this.$anime.timeline({
        easing: "linear",
        duration: this.animationSpeed,
        loop: true
      });

      this.automatedPositions.forEach(cords => {
        if (cords.x === 0 && cords.y === 0) {
          this.tl.add({
            targets: this.$data,
            x: 0,
            y: 0
          });
        } else {
          this.tl.add({
            targets: this.$data,
            x: this.getXPos(cords.x),
            y: this.getXPos(cords.y)
          });
        }
      });
      this.ticker = new PIXI.Ticker();
      this.ticker.add(() => {
        this.displacementFilter.scale.x = this.x;
        this.displacementFilter.scale.y = this.y;
      });
      this.ticker.start();
    }
  },
  methods: {
    setScale() {
      this.containerRatio = this.width / this.height;
      this.imageRatio = this.imgTexture.width / this.imgTexture.height;
      if (this.containerRatio > this.imageRatio) {
        this.scale = this.width / this.img.width;
      } else {
        this.scale = this.height / this.img.height;
      }
      this.setWidthHeight();
    },
    setWidthHeight() {
      this.img.x = this.app.renderer.screen.width / 2;
      this.img.y = this.app.renderer.screen.height / 2;
      this.img.scale.x = this.scale;
      this.img.scale.y = this.scale;
      this.depthMapCont.x = this.app.renderer.screen.width / 2;
      this.depthMapCont.y = this.app.renderer.screen.height / 2;
      this.depthMapCont.scale.x = this.scale;
      this.depthMapCont.scale.y = this.scale;
    },
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
  height: 100vh;
  width: 100vw;
  h1 {
    color: blue;
  }
}
</style>
