<template>
  <div class="modal-container">
    <strong>Enter Website URL</strong>
    <div class="input-set">
      <input
        ref="inp"
        type="text"
        :style="{ 'background-color': selectedGradient.start }"
      />
      <button :style="{ 'background-color': selectedGradient.start }">
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
export default {
  name: "InputModal",
  props: {
    selectedGradient: {
      type: Object,
    },
  },
  mounted() {
    this.$refs.inp.focus();
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
</style>