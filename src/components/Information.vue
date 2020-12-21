<template>
  <transition name="author-transition">
    <div class="info-container" v-if="qod">
      <div
        class="container"
        @mouseenter="showAuthor = true"
        @mouseleave="showAuthor = false"
      >
        <b>&#8220; </b>
        <span>{{ qod }}</span>
        <b> &#8221;</b>
        <transition name="author-transition">
          <span class="author" v-if="showAuthor"> — {{ author }}</span>
        </transition>
      </div>
    </div>
  </transition>
</template>

<script>
import axios from "axios";
export default {
  name: "Information",
  data() {
    return {
      qod: null,
      author: null,
      showAuthor: false,
    };
  },
  mounted() {
    axios
      .get("https://api.quotable.io/random?maxLength=80")
      .then((response) => {
        this.qod = response.data.content;
        this.author = response.data.author;
      })
      .catch((error) => {
        console.error(error);
      });
  },
};
</script>

<style scoped>
.info-container {
  display: flex;
  justify-content: center;
}
.container {
  color: white;
  font-size: 18px;
  text-align: center;
  cursor: default;
  display: inline;
}

span {
  font-family: "DancingScript", Times, serif;
}
b {
  font-family: "Times New Roman", Times, serif;
}
.author {
  white-space: nowrap;
  position: absolute;
  margin-left: 0.5em;
  margin-top: 0.2em;
}
.author-transition-enter-active {
  transition: all 0.3s ease;
}
.author-transition-enter,
.author-transition-leave-to {
  opacity: 0;
}
</style>