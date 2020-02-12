<template>
  <div id="app">
    <Still3d :key="bg + num" :bgImage="bg" :depthMap="depth" :mouse="isMouse"/>
    <Video3d :key="num*10" :bgImage="video" :depthMap="videoDepth" :mouse="true"/>
    <div id="buttonContainer">
      <div>
        <button @click="setBackground('controller')">Controller</button>
        <button @click="setBackground('boarder')">Boarder</button>
        <button @click="setBackground('subway')">Subway</button>
        <button @click="setBackground('mountain')">Mountain</button>
        <button @click="setBackground('video')">Video</button>
      </div>
      <div>
        <button @click="updateMouse(false)">Auto</button>
        <button @click="updateMouse(true)">Mouse Move</button>
      </div>
    </div>
  </div>
</template>

<script>
import Still3d from "./components/Still3d";
import Video3d from "./components/Video3d";
import controller from "@/assets/controller.jpg";
import controllerDepth from "@/assets/controller-depth.jpg";
import boarder from "@/assets/boarder.jpg";
import boarderDepth from "@/assets/boarder-depth2.jpg";
import subway from "@/assets/subway.jpg";
import subwayDepth from "@/assets/subway-depth.jpg";
import mountain from "@/assets/mountain.jpg";
import mountainDepth from "@/assets/mountain-depth.jpg";
import video from "@/assets/video.mp4";
import videoDepth from "@/assets/video-depth.jpg";
export default {
  name: "App",
  components: {
    Still3d,
    Video3d
  },
  data() {
    return {
      bg: controller,
      depth: controllerDepth,
      video,
      videoDepth,
      num: 0,
      isMouse: false
    };
  },
  methods: {
    setBackground(val) {
      if (val === "boarder") {
        this.bg = boarder;
        this.depth = boarderDepth;
        this.num++;
      }
      if (val === "controller") {
        this.bg = controller;
        this.depth = controllerDepth;
        this.num++;
      }
      if (val === "subway") {
        this.bg = subway;
        this.depth = subwayDepth;
        this.num++;
      }
      if (val === "mountain") {
        this.bg = mountain;
        this.depth = mountainDepth;
        this.num++;
      }
      if (val === "video") {
        this.bg = video;
        this.depth = videoDepth;
        this.num++;
      }
    },
    updateMouse(bool) {
      this.isMouse = bool;
      this.num++;
    }
  }
};
</script>

<style lang="scss">
html,
body {
  margin: 0;
  padding: 0;
  max-width: 100vw;

}
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 0px;
  padding: 0px;
}

#buttonContainer {
  display: flex;
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  justify-content: space-between;
  align-items: space-between;
  > div {
    padding: 0px 30px;
  }
}
</style>
