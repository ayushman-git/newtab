<template>
  <div>
    <h2>Change Tab Size</h2>
    <TabSlider />
  </div>
</template>

<script>
import TabSlider from "./TabSlider";
export default {
  name: "UserSettings",
  components: { TabSlider },
};
</script>

<style scoped>
div {
  background-color: rgb(51, 51, 51);
}
</style>