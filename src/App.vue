<template>
  <div id="app">
    <Still3d
      :key="bg + num"
      :bgImage="bg"
      :depthMap="depth"
      :mouse="isMouse"
      :automatedPositions="automatedPositions"
      :animationSpeed="animationSpeed"
    />
    <div id="buttonContainer">
      <div>
        <span>Backgrounds:</span>
        <button
          @click="setBackground('controller')"
          :class="{ 'btn--active': currentBackground === 'controller' }"
        >
          Controller
        </button>
        <button
          @click="setBackground('boarder')"
          :class="{ 'btn--active': currentBackground === 'boarder' }"
        >
          Boarder
        </button>
        <button
          @click="setBackground('subway')"
          :class="{ 'btn--active': currentBackground === 'subway' }"
        >
          Subway
        </button>
        <button
          @click="setBackground('mountain')"
          :class="{ 'btn--active': currentBackground === 'mountain' }"
        >
          Mountain
        </button>
      </div>
      <div>
        <span>Interactions:</span>
        <button
          @click="updateMouse(false)"
          :class="{ 'btn--active': !isMouse }"
        >
          Auto
        </button>
        <button @click="updateMouse(true)" :class="{ 'btn--active': isMouse }">
          Mouse Move
        </button>
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
import subway from "@/assets/subway.jpg";
import subwayDepth from "@/assets/subway-depth.jpg";
import mountain from "@/assets/mountain.jpg";
import mountainDepth from "@/assets/mountain-depth-new.jpg";
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
      isMouse: false,
      currentBackground: "controller",
      automatedPositions: undefined,
      animationSpeed: undefined
    };
  },
  mounted() {
    this.setBackground("controller");
  },
  methods: {
    setBackground(val) {
      this.currentBackground = val;
      this.automatedPositions = undefined;
      this.animationSpeed = undefined;
      if (val === "boarder") {
        this.bg = boarder;
        this.depth = boarderDepth;
        this.animationSpeed = 1600;
      }
      if (val === "controller") {
        this.bg = controller;
        this.depth = controllerDepth;
        this.animationSpeed = 2500;
        this.automatedPositions = [
          {
            x: 0.02,
            y: 0.98
          },
          {
            x: 0.45,
            y: 0.01
          },
          {
            x: 0.99,
            y: 0.07
          },
          {
            x: 0.98,
            y: 0.97
          },
          {
            x: 0,
            y: 0
          }
        ];
      }
      if (val === "subway") {
        this.bg = subway;
        this.depth = subwayDepth;
        this.animationSpeed = 2500;
        this.automatedPositions = [
          {
            x: 0.97,
            y: 0.96
          },
          {
            x: 0.72,
            y: 0.08
          },
          {
            x: 0.03,
            y: 0.94
          },
          {
            x: 0,
            y: 0
          }
        ];
      }
      if (val === "mountain") {
        this.bg = mountain;
        this.depth = mountainDepth;
        this.animationSpeed = 2000;
        this.automatedPositions = [
          {
            x: 0.97,
            y: 0.07
          },
          {
            x: 0.92,
            y: 0.92
          },
          {
            x: 0.06,
            y: 0.86
          },
          {
            x: 0.04,
            y: 0.06
          },
          {
            x: 0.53,
            y: 0.48
          },
          {
            x: 0,
            y: 0
          }
        ];
      }
      this.num++;
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
  padding: 10px 0px;
  background: rgba(0, 0, 0, 0.5);
  backdrop-filter: blur(5px);
  > div {
    padding: 0px 30px;
    span {
      color: #fff;
      text-transform: uppercase;
      margin: 0;
    }
  }
  button {
    padding: 5px 10px;
    display: inline-block;
    border-radius: 3px;
    border: none;
    background: #ccc;
    font-size: 12px;
    font-weight: bold;
    color: #333;
    margin-left: 4px;
    margin-right: 4px;
    cursor: pointer;
    &:hover {
      background: rgb(39, 212, 172);
    }
    &.btn--active {
      background: rgb(92, 255, 217);
      &:hover {
        background: rgb(39, 212, 172);
      }
    }
  }
}
</style>
