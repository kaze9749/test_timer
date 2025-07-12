<template>
  <div id="app">
    <PcupTimer :threshold="30" /> <!-- 45秒に設定する例 -->
  </div>
</template>

<script>
import PcupTimer from "./components/PcupTimer.vue";

export default {
  name: "App",
  components: {
    PcupTimer,
  },
};
</script>

<style>
#app {
  margin: 0;
  padding: 0;
  overflow: hidden;
}
</style>
