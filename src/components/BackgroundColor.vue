<template>
  <div class="container">
    <div
      class="gradient-circle"
      v-for="gradient in localGradients"
      :key="gradient.index"
      :style="[{
        background: `linear-gradient(135deg, ${gradient.start}, ${gradient.end}`,
      },
        selectedGradient == gradient ? {
          'border': '5px solid rgb(107, 228, 153)'
        }:{
        }
      ]"
      @click="$emit('selectcolor', gradient)"
    ></div>
  </div>
</template>

<script>
import gradients from "../assets/gradients";
export default {
  name: "BackgroundColor",
  props: {
    selectedGradient: {
      type: Object,
    },
  },
  data() {
    return {
      localGradients: gradients,
    };
  },
};
</script>

<style scoped>
.container {
  padding-top: 20px;
  border-radius: 100px;
  background-color: white;
  width: 80vw;
  height: 140px;
  display: flex;
  justify-content: space-evenly;
}
.gradient-circle {
  cursor: pointer;
  width: 56px;
  height: 56px;
  border-radius: 50%;
  transition: 2 border cubic-bezier(0.075, 0.82, 0.165, 1);
}
</style>