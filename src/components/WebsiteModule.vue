<template>
  <div class="website-container" @click="goToLink">
    <img :src="websiteObj.icon" />
    <p>{{ websiteObj.title }}</p>
  </div>
</template>

<script>
export default {
  name: "WebsiteModule",
  props: {
    websiteObj: {
      type: Object,
    },
  },
  mounted() {
    console.log(this.websiteObj);
  },
  methods: {
    goToLink() {
      window.open(this.websiteObj.link, "_blank");
    },
  },
};
</script>

<style scoped>
.website-container {
  margin: 1em;
  min-width: 120px;
  min-height: 120px;
  background-color: rgba(0, 0, 0, 0.2);
  display: flex;
  flex-flow: column nowrap;
  align-items: center;
  justify-content: space-evenly;
  border-radius: 20px;
  cursor: pointer;
  transition: 0.2s all cubic-bezier(0.075, 0.82, 0.165, 1);
}
.website-container:hover {
  background-color: rgba(0, 0, 0, 0.3);
  box-shadow: 0px 0px 10px 0px rgba(0, 0, 0, 0.75);
}
.website-container:hover img {
  transform: scale(1.1);
}
img {
  width: 50px;
  height: 50px;
  transition: 0.2s all cubic-bezier(0.075, 0.82, 0.165, 1);
}

p {
  color: white;
  margin: 0;
  padding: 0;
}
</style>