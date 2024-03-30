<script>
import { ref } from "vue";
import DisplayOptionsList from "./DisplayOptionsList.vue";
import ResponsiveStateMap from "./ResponsiveStateMap.vue";
import ResponsiveCityMap from "./ResponsiveCityMap.vue";
import DisplaySelectionList from "./DisplaySelectionList.vue";
export default {
  name: "Home",
  components: {
    DisplayOptionsList,
    ResponsiveStateMap,
    ResponsiveCityMap,
    DisplaySelectionList,
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
    <div class="grid grid-cols-12 gap-4 content-start items-center">
      <div class="flex justify-center col-span-1 w-full">
        <display-options-list />
      </div>
      <div class="col-span-10 w-3/4">
        <h1>{{ description }}</h1>
        <responsive-state-map />
        <!-- <responsive-city-map /> -->
      </div>
      <div class="flex justify-center col-span-1 w-1/6">
        <display-selection-list />
      </div>
    </div>
  </div>
</template>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>