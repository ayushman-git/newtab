<template>
  <div class="modal-container">
    <div class="header">
      <strong>Enter Website URL</strong>
      <span class="delimiter">{{ randomEmoji() }}</span>
      <transition name="error">
        <span v-if="incorrectURL" class="error-msg">Incorrect URL</span>
      </transition>
    </div>
    <div class="input-set">
      <input
        v-model="url"
        ref="inp"
        type="text"
        :style="{ 'background-color': selectedGradient.start }"
        @keyup.enter="fetchIcons"
      />
      <button
        @click="fetchIcons"
        :style="{ 'background-color': selectedGradient.start }"
      >
        <svg
          xmlns="http://www.w3.org/2000/svg"
          height="50"
          viewBox="0 0 24 24"
          width="50"
        >
          <path d="M0 0h24v24H0z" fill="none" />
          <path d="M9 16.2L4.8 12l-1.4 1.4L9 19 21 7l-1.4-1.4L9 16.2z" />
        </svg>
      </button>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "InputModal",
  props: {
    selectedGradient: {
      type: Object,
    },
  },
  data() {
    return {
      url: "",
      incorrectURL: false,
      emojis: [
        "😋",
        "😮",
        "🤐",
        "😪",
        "😵",
        "🤯",
        "😁",
        "🤣",
        "😐",
        "😶",
        "🙄",
        "🤗",
        "😙",
        "😱",
        "🤕",
        "😓",
        "🥳",
        "😎",
        "🤓",
        "🥴",
        "🤪",
        "😴",
        "🥺",
        "😒",
        "🤩",
        "😍",
        "😉",
      ],
    };
  },
  mounted() {
    this.$refs.inp.focus();
  },
  computed: {
    validURL() {
      var pattern = new RegExp(
        "^(https?:\\/\\/)?" + // protocol
          "((([a-z\\d]([a-z\\d-]*[a-z\\d])*)\\.)+[a-z]{2,}|" + // domain name
          "((\\d{1,3}\\.){3}\\d{1,3}))" + // OR ip (v4) address
          "(\\:\\d+)?(\\/[-a-z\\d%_.~+]*)*" + // port and path
          "(\\?[;&a-z\\d%_.~+=-]*)?" + // query string
          "(\\#[-a-z\\d_]*)?$",
        "i"
      ); // fragment locator
      return !!pattern.test(this.url);
    },
  },
  methods: {
    randomEmoji() {
      return this.emojis[Math.floor(Math.random() * this.emojis.length)];
    },
    fetchIcons() {
      if (this.validURL) {
        this.$emit("closeModal");

        this.$emit(
          "addWebsite",
          "https://logo.clearbit.com/" + this.url,
          this.url
        );

        this.fetchIconsFromAPI();
      } else {
        this.incorrectURL = true;
        setTimeout(() => {
          this.incorrectURL = false;
        }, 3500);
      }
    },
    fetchIconsFromAPI() {
      axios
        .post(
          "https://logo-scraping-api.herokuapp.com/url",
          {
            url: this.url,
          },
          {
            headers: {
              "Content-Type": "application/json",
            },
          }
        )
        .then((res) => {
          this.$emit(
            "addMoreIcons",
            res.data.map((singleUrlObj) => {
              return singleUrlObj.url;
            })
          );
          this.url = "";
        });
    },
  },
};
</script>

<style scoped>
strong {
  color: white;
}
.modal-container {
  backdrop-filter: blur(10px);
  z-index: 10;
  background-color: black;
  width: 50%;
  padding: 2em;
  box-shadow: 0px 0px 60px 0px rgba(0, 0, 0, 1);
}
.input-set {
  margin-top: 1em;
  display: flex;
  justify-content: flex-start;
}
input {
  padding: 1em;
  font-size: 20px;
  border-radius: 15px;
  width: 100%;
  margin-right: 2em;
  height: 4em;
  outline: none;
  border: none;
}
button {
  outline: none;
  border-radius: 50%;
  min-height: 80px;
  min-width: 80px;
  max-height: 80px;
  max-width: 80px;
  border: none;
  cursor: pointer;
  transition: 0.4s all cubic-bezier(0.165, 0.84, 0.44, 1);
}
button:hover {
  transform: scale(1.2);
}
svg {
  margin-top: 7px;
}
.error-msg {
  color: white;
  font-weight: 600;
}
.header > * {
  margin-right: 1em;
}
.error-enter-active,
.error-leave-active {
  transition: all 0.3s ease;
}
.error-enter,
.error-leave-to {
  opacity: 0;
}
</style>