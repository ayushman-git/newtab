<template>
  <div
    @click="emitModal"
    class="website-container"
    :style="[!websiteObj ? { 'align-items': 'center' } : {}]"
    @mouseenter="showCloseFunc"
    @mouseleave="[(showClose = false), preventShowCloseFunc()]"
  >
    <a :href="websiteObj.link" v-if="websiteObj" class="content-container">
      <img :src="websiteObj.icons[websiteObj.selectedIconIndex]" />
      <p>{{ websiteObj.title }}</p>
    </a>
    <div class="content-container" v-if="!websiteObj">
      <svg
        xmlns="http://www.w3.org/2000/svg"
        height="50"
        viewBox="0 0 24 24"
        width="50"
      >
        <path d="M0 0h24v24H0z" fill="none" />
        <path
          :fill="selectedGradient.start"
          d="M19 13h-6v6h-2v-6H5v-2h6V5h2v6h6v2z"
        />
      </svg>
    </div>
    <transition name="close-anim">
      <div @click="removeWebsite" v-if="showClose && websiteObj" class="close">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          height="24"
          viewBox="0 0 24 24"
          width="24"
        >
          <path d="M0 0h24v24H0z" fill="none" />
          <path
            :fill="selectedGradient.start"
            d="M19 6.41L17.59 5 12 10.59 6.41 5 5 6.41 10.59 12 5 17.59 6.41 19 12 13.41 17.59 19 19 17.59 13.41 12z"
          />
        </svg></div
    ></transition>
    <transition name="close-anim">
      <div
        v-if="showClose && websiteObj"
        class="refresh"
        @click="refresh"
      >
        <svg
          xmlns="http://www.w3.org/2000/svg"
          height="24"
          viewBox="0 0 24 24"
          width="24"
        >
          <path d="M0 0h24v24H0z" fill="none" />
          <path
            :fill="selectedGradient.start"
            d="M19 8l-4 4h3c0 3.31-2.69 6-6 6-1.01 0-1.97-.25-2.8-.7l-1.46 1.46C8.97 19.54 10.43 20 12 20c4.42 0 8-3.58 8-8h3l-4-4zM6 12c0-3.31 2.69-6 6-6 1.01 0 1.97.25 2.8.7l1.46-1.46C15.03 4.46 13.57 4 12 4c-4.42 0-8 3.58-8 8H1l4 4 4-4H6z"
          />
        </svg></div
    ></transition>
  </div>
</template>

<script>
export default {
  name: "WebsiteModule",
  props: {
    websiteObj: {
      type: Object,
    },
    selectedGradient: {
      type: Object,
    },
  },
  data() {
    return {
      showClose: false,
      closeTM: null,
    };
  },
  methods: {
    showCloseFunc() {
      this.closeTM = setTimeout(() => {
        this.showClose = true;
      }, 800);
    },
    preventShowCloseFunc() {
      clearTimeout(this.closeTM);
    },
    emitModal() {
      if (!this.websiteObj) {
        this.$emit("modalClicked");
      }
    },
    removeWebsite() {
      this.$emit("remove", this.websiteObj);
    },
    refresh() {
      this.$emit('refresh', this.websiteObj)
    }
  },
};
</script>

<style scoped>
a {
  text-decoration: none;
}
a:active {
  text-decoration: none;
}
.link-container {
  display: block;
}
.content-container {
  width: 100%;
  display: flex;
  flex-flow: column nowrap;
  align-items: center;
  justify-content: space-evenly;
}
.website-container {
  position: relative;
  margin: 1em;
  max-width: 140px;
  min-height: 120px;
  display: flex;
  justify-content: center;
  background-color: rgba(0, 0, 0, 0.2);
  border-radius: 20px;
  cursor: pointer;
  transition: 0.2s all cubic-bezier(0.075, 0.82, 0.165, 1);
}
.website-container:hover {
  background-color: rgba(0, 0, 0, 0.3);
  box-shadow: 0px 0px 10px 0px rgba(0, 0, 0, 0.75);
}
.website-container:hover img {
  transform: scale(1.05);
}
img {
  max-height: 30px;
  max-width: 80px;
  transition: 0.2s all cubic-bezier(0.075, 0.82, 0.165, 1);
}

p {
  color: white;
  margin: 0;
  padding: 0;
  font-size: 14px;
  font-weight: 400;
}
.close {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 35px;
  height: 35px;
  border-radius: 50%;
  background-color: rgba(0, 0, 0, 0.8);
  position: absolute;
  top: -10px;
  right: -10px;
}
.refresh {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 35px;
  height: 35px;
  border-radius: 50%;
  background-color: rgba(0, 0, 0, 0.8);
  position: absolute;
  top: -10px;
  left: -10px;
}

.close-anim-enter-active {
  transition: all 0.3s ease;
}
.close-anim-enter,
.close-anim-leave-to {
  transform: scale(0.5);
  opacity: 0;
}
</style>