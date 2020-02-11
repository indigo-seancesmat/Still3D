<template>
  <div id="app">
    <Still3d :key="bg + num" :bgImage="bg" :depthMap="depth" :mouse="isMouse"/>
    <div id="buttonContainer">
      <div>
        <button @click="setBackground('boarder')">Boarder</button>
        <button @click="setBackground('controller')">Controller</button>
      </div>
      <div>
        <button @click="updateMouse(true)">Mouse Move</button>
        <button @click="updateMouse(false)">Auto</button>
      </div>
    </div>
  </div>
</template>

<script>
import Still3d from "./components/Still3d";
import controller from "@/assets/controller.jpg";
import controllerDepth from "@/assets/controller-depth.jpg";
import boarder from "@/assets/boarder.jpg";
import boarderDepth from "@/assets/boarder-depth2.jpg";
export default {
  name: "App",
  components: {
    Still3d
  },
  data() {
    return {
      bg: controller,
      depth: controllerDepth,
      num: 0,
      isMouse: false
    };
  },
  methods: {
    setBackground(val) {
      if (val === "boarder") {
        this.bg = boarder;
        this.depth = boarderDepth;
      }
      if (val === "controller") {
        this.bg = controller;
        this.depth = controllerDepth;
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
