<template>
  <div
    @mouseenter="hover = true"
    @mouseleave="hover = false"
    @click="emitLockStatus"
  >
    <svg
      class="lock-icon"
      version="1.1"
      id="Capa_1"
      xmlns="http://www.w3.org/2000/svg"
      xmlns:xlink="http://www.w3.org/1999/xlink"
      x="0px"
      y="0px"
      viewBox="0 0 448 448"
      style="enable-background: new 0 0 448 448"
      xml:space="preserve"
      :fill="hover || lock ? 'black' : selectedGradient.start"
    >
      <g>
        <g>
          <path
            d="M352,149.333h-21.333v-42.667C330.667,47.787,282.88,0,224,0S117.333,47.787,117.333,106.667v42.667H96
			c-23.573,0-42.667,19.093-42.667,42.667v213.333C53.333,428.907,72.427,448,96,448h256c23.573,0,42.667-19.093,42.667-42.667V192
			C394.667,168.427,375.573,149.333,352,149.333z M224,341.333c-23.573,0-42.667-19.093-42.667-42.667
			C181.333,275.093,200.427,256,224,256s42.667,19.093,42.667,42.667C266.667,322.24,247.573,341.333,224,341.333z M290.133,149.333
			H157.867v-42.667c0-36.48,29.653-66.133,66.133-66.133s66.133,29.653,66.133,66.133V149.333z"
          />
        </g>
      </g>
    </svg>
  </div>
</template>

<script>
export default {
  name: "Lock",
  props: {
    selectedGradient: {
      type: Object,
    },
    lock: {
      type: Boolean,
    },
  },
  data() {
    return {
      hover: false,
    };
  },
  methods: {
    emitLockStatus() {
      this.$emit("lockStatus");
    },
  },
};
</script>

<style scoped>
div {
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
div:hover {
  box-shadow: 0 0px 2.2px rgba(0, 0, 0, 0.042), 0 0px 5.3px rgba(0, 0, 0, 0.059),
    0 0px 9.9px rgba(0, 0, 0, 0.071), 0 0px 17.6px rgba(0, 0, 0, 0.083),
    0 0px 33px rgba(0, 0, 0, 0.1), 0 0px 79px rgba(0, 0, 0, 0.14);
  background-color: rgb(255, 255, 255);
}

div > svg {
  transition: all 0.2 ease-in-out;
}
.lock-icon {
  width: 20px;
}
</style>