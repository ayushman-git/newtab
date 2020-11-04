<template>
  <div
    :style="{
      background: `linear-gradient(180deg, ${gradient.start}, ${gradient.end}`,
    }"
  ></div>
</template>

<script>
export default {
  name: "Background",
  props: {
    gradient: {
      type: Object,
      required: true,
    },
  },
};
</script>

<style scoped>
div {
  width: 100%;
  height: 100vh;
}
</style>