<template>
  <div
    id="app"
    :style="{
      background: `linear-gradient(180deg, ${selectedGradient.start}, ${selectedGradient.end}`,
    }"
  >
    <Weather class="weather" :lock="lock" />
    <Information class="information" />
    <draggable v-model="initialWebsites" @change="saveData">
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
          v-on:refresh="refresh"
          :tabBorderRadius="tabBorderRadius"
          :lock="lock"
        />
        <WebsiteModule
          v-on:modalClicked="showModal = true"
          :key="99"
          :selectedGradient="selectedGradient"
          :tabBorderRadius="tabBorderRadius"
        />
      </transition-group>
    </draggable>
    <transition name="bg-selector">
      <div class="color-selector-container" v-if="!lock">
        <BackgroundColor
          :selectedGradient="selectedGradient"
          v-on:selectcolor="changeGradient"
          class="color-selector"
        />
      </div>
    </transition>
    <transition name="input-anim">
      <!-- must be v-show -->
      <InputModal
        v-on:addMoreIcons="addMoreIcons"
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
    <div class="setting-pins">
      <!-- <transition name="setting-anim">
        <div
          v-if="!lock"
          class="setting-icon"
          @click="showUserSettings = true"
          @mouseenter="hoverSetting = true"
          @mouseleave="hoverSetting = false"
        >
          <svg
            class="setting-icon-svg"
            enable-background="new 0 0 24 24"
            height="512"
            viewBox="0 0 24 24"
            width="512"
            xmlns="http://www.w3.org/2000/svg"
            :fill="hoverSetting ? 'blacke' : selectedGradient.start"
          >
            <path
              d="m22.683 9.394-1.88-.239c-.155-.477-.346-.937-.569-1.374l1.161-1.495c.47-.605.415-1.459-.122-1.979l-1.575-1.575c-.525-.542-1.379-.596-1.985-.127l-1.493 1.161c-.437-.223-.897-.414-1.375-.569l-.239-1.877c-.09-.753-.729-1.32-1.486-1.32h-2.24c-.757 0-1.396.567-1.486 1.317l-.239 1.88c-.478.155-.938.345-1.375.569l-1.494-1.161c-.604-.469-1.458-.415-1.979.122l-1.575 1.574c-.542.526-.597 1.38-.127 1.986l1.161 1.494c-.224.437-.414.897-.569 1.374l-1.877.239c-.753.09-1.32.729-1.32 1.486v2.24c0 .757.567 1.396 1.317 1.486l1.88.239c.155.477.346.937.569 1.374l-1.161 1.495c-.47.605-.415 1.459.122 1.979l1.575 1.575c.526.541 1.379.595 1.985.126l1.494-1.161c.437.224.897.415 1.374.569l.239 1.876c.09.755.729 1.322 1.486 1.322h2.24c.757 0 1.396-.567 1.486-1.317l.239-1.88c.477-.155.937-.346 1.374-.569l1.495 1.161c.605.47 1.459.415 1.979-.122l1.575-1.575c.542-.526.597-1.379.127-1.985l-1.161-1.494c.224-.437.415-.897.569-1.374l1.876-.239c.753-.09 1.32-.729 1.32-1.486v-2.24c.001-.757-.566-1.396-1.316-1.486zm-10.683 7.606c-2.757 0-5-2.243-5-5s2.243-5 5-5 5 2.243 5 5-2.243 5-5 5z"
            />
          </svg>
        </div>
      </transition> -->

      <Lock
        :lock="lock"
        :selectedGradient="selectedGradient"
        v-on:lockStatus="lock = !lock"
      />
    </div>
    <UserSettings v-if="showUserSettings" class="user-setting" />
    <div
      @click="showUserSettings = false"
      v-if="showUserSettings"
      class="back-drop"
    ></div>
  </div>
</template>

<script>
import draggable from "vuedraggable";

import BackgroundColor from "./components/BackgroundColor";
import Weather from "./components/Weather";
import Information from "./components/Information";
import WebsiteModule from "./components/WebsiteModule";
import InputModal from "./components/InputModal";
import Lock from "./components/Lock";
import UserSettings from "./components/UserSettings";
export default {
  name: "App",
  components: {
    BackgroundColor,
    Weather,
    Information,
    WebsiteModule,
    InputModal,
    draggable,
    Lock,
    UserSettings,
  },
  created() {
    const notFirstTime = !!localStorage.getItem("gradient");
    const notFirstTimeWebsite = !!localStorage.getItem("initialWebsites");
    const notFirstTimeLock = !!localStorage.getItem("lock");
    if (notFirstTime) {
      this.selectedGradient = JSON.parse(localStorage.getItem("gradient"));
    } else {
      this.selectedGradient = {
        start: "#222222",
        end: "#000000",
      };
    }
    if (notFirstTimeLock) {
      this.lock = JSON.parse(localStorage.getItem("lock"));
    } else {
      this.lock = false;
    }
    if (notFirstTimeWebsite) {
      this.initialWebsites = JSON.parse(
        localStorage.getItem("initialWebsites")
      );
    } else {
      this.initialWebsites = [
        {
          title: "Youtube",
          icons: [
            "https://www.flaticon.com/svg/static/icons/svg/1384/1384060.svg",
            "https://i.pinimg.com/originals/0b/20/04/0b2004499710b7b222644edaa60f7711.png",
          ],
          selectedIconIndex: 0,
          link: "https://www.youtube.com",
        },
        {
          title: "Facebook",
          icons: [
            "https://www.flaticon.com/svg/static/icons/svg/733/733549.svg",
          ],
          selectedIconIndex: 0,
          link: "https://www.facebook.com",
        },
        {
          title: "Twitter",
          icons: [
            "https://www.flaticon.com/svg/static/icons/svg/733/733579.svg",
          ],
          selectedIconIndex: 0,
          link: "https://www.twitter.com",
        },
      ];
    }
  },
  data() {
    return {
      selectedGradient: null,
      showModal: false,
      initialWebsites: null,
      lock: false,
      showUserSettings: false,
      hoverSetting: false,
      tabBorderRadius: 20,
    };
  },
  computed: {
    dragOptions() {
      return {
        animation: 200,
        disabled: false,
        ghostClass: "ghost",
      };
    },
  },
  methods: {
    changeGradient(a) {
      this.selectedGradient = a;
      localStorage.setItem("gradient", JSON.stringify(this.selectedGradient));
    },
    addWebsite(iconURLs, url) {
      const webName = url.replace(/.+\/\/|www.|\..+/g, "");
      const webNameCaps = webName.charAt(0).toUpperCase() + webName.slice(1);
      const webObj = {
        title: webNameCaps,
        icons: [iconURLs],
        selectedIconIndex: 0,
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
    refresh(changedIndex) {
      this.initialWebsites.forEach((site) => {
        if (JSON.stringify(site) === JSON.stringify(changedIndex)) {
          if (site.icons.length - 1 === site.selectedIconIndex) {
            site.selectedIconIndex = 0;
          } else {
            site.selectedIconIndex++;
          }
        }
      });
      localStorage.setItem(
        "initialWebsites",
        JSON.stringify(this.initialWebsites)
      );
    },
    addMoreIcons(moreIcons) {
      console.log(this.initialWebsites);

      this.initialWebsites[this.initialWebsites.length - 1].icons.push(
        ...moreIcons
      );
      localStorage.setItem(
        "initialWebsites",
        JSON.stringify(this.initialWebsites)
      );
    },
    saveData() {
      const filtered = this.initialWebsites.filter(function (el) {
        return el != null;
      });
      this.initialWebsites = filtered;
      localStorage.setItem(
        "initialWebsites",
        JSON.stringify(this.initialWebsites)
      );
    },
  },
  watch: {
    lock: function () {
      localStorage.setItem("lock", this.lock);
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
@font-face {
  font-family: DancingScript;
  src: url("assets/fonts/DancingScript.ttf");
}
.weather {
  position: fixed;
  top: 25px;
  left: 25px;
  user-select: none;
}
.information {
  margin: 0 auto;
  padding: 20vh 1em 4em 1em;
  user-select: none;
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
  position: absolute;
  top: 30%;
  left: 50%;
  transform: translateX(-50%);
  display: grid;
  grid-gap: 1em;
  grid-template-columns: repeat(auto-fit, minmax(120px, 160px));
  justify-content: center;
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

.bg-selector-enter-active,
.bg-selector-leave-active {
  transition: all 0.4s ease;
}
.bg-selector-enter,
.bg-selector-leave-to {
  transform: translateY(50px);
  opacity: 0;
}

.setting-anim-enter-active,
.setting-anim-leave-active {
  transition: all 0.2s ease;
}
.setting-anim-enter,
.setting-anim-leave-to {
  transform: translateY(50px);
  z-index: -1;
  opacity: 0;
}

.setting-pins {
  position: fixed;
  bottom: 30px;
  right: 30px;
  display: flex;
  flex-flow: column;
}
.setting-pins > * {
  margin-top: 1.4em;
}
.setting-icon {
  display: flex;
  width: 40px;
  height: 40px;
  background-color: rgb(218, 218, 218);
  border-radius: 50%;
  justify-content: center;
  align-items: center;
  transition: all 0.2s ease-in-out;
  cursor: pointer;
}
.setting-icon:hover {
  box-shadow: 0 0px 2.2px rgba(0, 0, 0, 0.042), 0 0px 5.3px rgba(0, 0, 0, 0.059),
    0 0px 9.9px rgba(0, 0, 0, 0.071), 0 0px 17.6px rgba(0, 0, 0, 0.083),
    0 0px 33px rgba(0, 0, 0, 0.1), 0 0px 79px rgba(0, 0, 0, 0.14);
  background-color: rgb(255, 255, 255);
}

.setting-icon-svg {
  width: 20px;
  transition: all 0.2s ease-in-out;
}
.user-setting {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 80%;
  max-width: 1000px;
  height: 80vh;
  z-index: 10;
}
.back-drop {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100vh;
  background-color: rgba(0, 0, 0, 0.8);
  backdrop-filter: blur(4px);
  z-index: 5;
}
</style>
