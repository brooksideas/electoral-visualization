<script>
import { ref } from "vue";
import ResponsiveStateMap from "./ResponsiveStateMap.vue";
import ResponsiveCityMap from "./ResponsiveCityMap.vue";
export default {
  name: "Home",
  components: {
    ResponsiveStateMap,
    ResponsiveCityMap
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
    <!-- <responsive-state-map /> -->
    <responsive-city-map />
  </div>
</template>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>