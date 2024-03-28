<script>
import { ref } from "vue";
import ResponsiveLineChart from "./ResponsiveLineChart.vue";
export default {
  name: "Home",
  components: {
    ResponsiveLineChart,
  },
  setup() {
    const description = ref("Electoral Visualization");
    return {
      description,
    };
  },
};
</script>

<template>
  <div id="app">
    <h1>{{ description }}</h1>
    <responsive-line-chart />
  </div>
</template>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>