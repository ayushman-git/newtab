<template>
  <div id="app">
    <Background :gradient="selectedGradient" />
    <div class="color-selector-container">
      <BackgroundColor :selectedGradient="selectedGradient" v-on:selectcolor="changeGradient" class="color-selector" />
    </div>
  </div>
</template>

<script>
import Background from "./components/Background";
import BackgroundColor from "./components/BackgroundColor";
export default {
  name: "App",
  components: {
    Background,
    BackgroundColor,
  },
  data() {
    return {
      selectedGradient: {
        start: "#222222",
        end: "#000000",
      },
    };
  },
  methods: {
    changeGradient(a) {
      this.selectedGradient = a;
    }
  }
};
</script>

<style>
* {
  box-sizing: border-box;
}
body {
  margin: 0;
  padding: 0;
  overflow: hidden;
}
.color-selector-container {
  padding-top: 50px;
  left: 50%;
  transform: translateX(-50%);
  position: absolute;
  bottom: -130px;
  transition: 0.2s bottom cubic-bezier(0.075, 0.82, 0.165, 1);
}
.color-selector-container:hover {
  bottom: -60px;
}
</style>
