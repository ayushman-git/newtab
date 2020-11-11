<template>
  <div
    id="app"
    :style="{
      background: `linear-gradient(180deg, ${selectedGradient.start}, ${selectedGradient.end}`,
    }"
  >
    <Weather class="weather" />
    <Information class="information" />
    <transition-group
      name="website-anim"
      class="website-module-container"
      tag="div"
    >
      <WebsiteModule
        v-for="(item, index) in initialWebsites"
        :key="index"
        :selectedGradient="selectedGradient"
        :websiteObj="item"
        v-on:remove="removeWebsite"
      />
      <WebsiteModule
        v-on:modalClicked="showModal = true"
        :key="99"
        :selectedGradient="selectedGradient"
      />
    </transition-group>
    <div class="color-selector-container">
      <BackgroundColor
        :selectedGradient="selectedGradient"
        v-on:selectcolor="changeGradient"
        class="color-selector"
      />
    </div>
    <transition name="input-anim">
      <InputModal
        v-on:addWebsite="addWebsite"
        v-show="showModal"
        class="input-modal"
        :selectedGradient="selectedGradient"
        v-on:closeModal="showModal = false"
      />
    </transition>
    <transition name="input-anim">
      <div @click="showModal = false" v-if="showModal" class="modal-bg"></div>
    </transition>
  </div>
</template>

<script>
import BackgroundColor from "./components/BackgroundColor";
import Weather from "./components/Weather";
import Information from "./components/Information";
import WebsiteModule from "./components/WebsiteModule";
import InputModal from "./components/InputModal";
export default {
  name: "App",
  components: {
    BackgroundColor,
    Weather,
    Information,
    WebsiteModule,
    InputModal,
  },
  created() {
    const notFirstTime = !!localStorage.getItem("gradient");
    const notFirstTimeWebsite = !!localStorage.getItem("initialWebsites");
    if (notFirstTime) {
      this.selectedGradient = JSON.parse(localStorage.getItem("gradient"));
    } else {
      this.selectedGradient = {
        start: "#222222",
        end: "#000000",
      };
    }
    if (notFirstTimeWebsite) {
      this.initialWebsites = JSON.parse(
        localStorage.getItem("initialWebsites")
      );
    } else {
      this.initialWebsites = [
        {
          title: "Youtube",
          icon:
            "https://www.flaticon.com/svg/static/icons/svg/1384/1384060.svg",
          link: "https://www.youtube.com",
        },
        {
          title: "Facebook",
          icon: "https://www.flaticon.com/svg/static/icons/svg/733/733549.svg",
          link: "https://www.facebook.com",
        },
        {
          title: "Twitter",
          icon: "https://www.flaticon.com/svg/static/icons/svg/733/733579.svg",
          link: "https://www.twitter.com",
        },
        {
          title: "Gmail",
          icon: "https://www.flaticon.com/svg/static/icons/svg/732/732200.svg",
          link: "https://www.gmail.com",
        },
      ];
    }
  },
  data() {
    return {
      selectedGradient: null,
      showModal: false,
      initialWebsites: null,
    };
  },
  methods: {
    changeGradient(a) {
      this.selectedGradient = a;
      localStorage.setItem("gradient", JSON.stringify(this.selectedGradient));
    },
    addWebsite(iconURLs, url) {
      console.log(iconURLs);
      const webName = url.replace(/.+\/\/|www.|\..+/g, "");
      const webNameCaps = webName.charAt(0).toUpperCase() + webName.slice(1);
      const webObj = {
        title: webNameCaps,
        icon: iconURLs,
        link: url,
      };
      this.initialWebsites.push(webObj);
      localStorage.setItem(
        "initialWebsites",
        JSON.stringify(this.initialWebsites)
      );
    },
    removeWebsite(deleteThisWebsite) {
      this.initialWebsites = this.initialWebsites.filter((site) => {
        return JSON.stringify(site) != JSON.stringify(deleteThisWebsite);
      });
      localStorage.setItem(
        "initialWebsites",
        JSON.stringify(this.initialWebsites)
      );
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
  width: 70%;
  position: relative;
  left: 50%;
  transform: translateX(-50%);
  display: grid;
  grid-gap: 1em;
  grid-template-columns: repeat(auto-fit, minmax(120px, 160px));
}
.website-add {
  width: 50px;
  height: 50px;
}
.input-modal {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  border-radius: 40px;
}
.modal-bg {
  z-index: 5;
  backdrop-filter: blur(50px);
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100vh;
  background-color: rgba(0, 0, 0, 0.4);
}
.input-anim-enter-active,
.input-anim-leave-active {
  transition: all 0.2s ease;
}
.input-anim-enter,
.input-anim-leave-to {
  opacity: 0;
}

.website-anim-enter-active,
.website-anim-leave-active {
  transition: all 0.4s ease;
}
.website-anim-enter,
.website-anim-leave-to {
  transform: scale(0.5);
  opacity: 0;
}
</style>
