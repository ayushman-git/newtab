<template>
  <div id="app">
    <Background :gradient="selectedGradient" />
    <Weather class="weather" />
    <Information class="information" />
    <div class="color-selector-container">
      <BackgroundColor
        :selectedGradient="selectedGradient"
        v-on:selectcolor="changeGradient"
        class="color-selector"
      />
    </div>
  </div>
</template>

<script>
import Background from "./components/Background";
import BackgroundColor from "./components/BackgroundColor";
import Weather from "./components/Weather";
import Information from "./components/Information"
export default {
  name: "App",
  components: {
    Background,
    BackgroundColor,
    Weather,
    Information
  },
  created() {
    const notFirstTime = !!(localStorage.getItem("gradient"));
    if (notFirstTime) {
      this.selectedGradient = JSON.parse(localStorage.getItem("gradient"));
    } else {
      this.selectedGradient = {
        start: "#222222",
        end: "#000000",
      };
    }
  },
  data() {
    return {
      selectedGradient: null,
    };
  },
  methods: {
    changeGradient(a) {
      this.selectedGradient = a;
      localStorage.setItem("gradient", JSON.stringify(this.selectedGradient));
    },
  },
};
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Open+Sans:wght@400;600;700;800&display=swap');
* {
  box-sizing: border-box;
  font-family: 'Open Sans', sans-serif;
}
.weather {
  position: absolute;
  top: 25px;
  left: 25px;
}
.information {
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
  top: 20%;
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
