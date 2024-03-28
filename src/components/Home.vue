<script>
import { ref } from "vue";
import ResponsiveMap from "./ResponsiveMap.vue";
export default {
  name: "Home",
  components: {
    ResponsiveMap,
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
    <responsive-map />
  </div>
</template>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>