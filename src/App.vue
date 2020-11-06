<template>
  <div
    id="app"
    :style="{
      background: `linear-gradient(180deg, ${selectedGradient.start}, ${selectedGradient.end}`,
    }"
  >
    <Weather class="weather" />
    <Information class="information" />
    <div class="website-module-container">
      <WebsiteModule :websiteObj="websiteObj" />
      <WebsiteModule :websiteObj="websiteObj" />
      <WebsiteModule :websiteObj="websiteObj" />
      <WebsiteModule :websiteObj="websiteObj" />
      <WebsiteModule :websiteObj="websiteObj" />
      <WebsiteModule :websiteObj="websiteObj" />
      <WebsiteModule :websiteObj="websiteObj" />
      <WebsiteModule :websiteObj="websiteObj" />
    </div>
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
import BackgroundColor from "./components/BackgroundColor";
import Weather from "./components/Weather";
import Information from "./components/Information";
import WebsiteModule from "./components/WebsiteModule";
export default {
  name: "App",
  components: {
    BackgroundColor,
    Weather,
    Information,
    WebsiteModule,
  },
  created() {
    const notFirstTime = !!localStorage.getItem("gradient");
    if (notFirstTime) {
      this.selectedGradient = JSON.parse(localStorage.getItem("gradient"));
    } else {
      this.selectedGradient = {
        start: "#222222",
        end: "#000000",
      };
    }
    console.log(this.selectedGradient)
  },
  data() {
    return {
      selectedGradient: null,
      websiteObj: {
        title: "Youtube",
        icon: "https://www.flaticon.com/svg/static/icons/svg/1384/1384060.svg",
        link: "https://www.youtube.com",
      },
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
@import url("https://fonts.googleapis.com/css2?family=Open+Sans:wght@400;600;700;800&display=swap");
* {
  box-sizing: border-box;
  font-family: "Open Sans", sans-serif;
}
#app {
  width: 100%;
  height: 100vh;
}
.weather {
  position: fixed;
  top: 25px;
  left: 25px;
}
.information {
  margin: 0 auto;
  padding: 20vh 1em 4em 1em;
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
  position: fixed;
  bottom: -130px;
  transition: 0.2s bottom cubic-bezier(0.075, 0.82, 0.165, 1);
}
.color-selector-container:hover {
  bottom: -60px;
}
.website-module-container {
  width: 80%;
  position: relative;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  flex-flow: row wrap;
  justify-content: space-evenly;
}
</style>
