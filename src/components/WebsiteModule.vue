<template>
  <div
    @click="emitModal"
    class="website-container"
    :style="[!websiteObj ? { 'align-items': 'center' } : {}]"
    @mouseenter="showCloseFunc"
    @mouseleave="[(showClose = false), preventShowCloseFunc()]"
  >
    <div v-if="websiteObj" class="content-container" @click="goToLink">
      <img :src="websiteObj.icon" />
      <p>{{ websiteObj.title }}</p>
    </div>
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
      <div v-if="showClose && websiteObj" class="close">
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
      tm: null,
    };
  },
  methods: {
    goToLink() {
      window.open(this.websiteObj.link, "_blank");
    },
    showCloseFunc() {
      this.tm = setTimeout(() => {
        this.showClose = true;
      }, 800);
    },
    preventShowCloseFunc() {
      clearTimeout(this.tm);
    },
    emitModal() {
      if(!this.websiteObj) {
        this.$emit('modalClicked')
      }
    },
  },
};
</script>

<style scoped>
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
  height: 30px;
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

.close-anim-enter-active {
  transition: all 0.3s ease;
}
.close-anim-enter,
.close-anim-leave-to {
  transform: scale(0.5);
  opacity: 0;
}
</style>