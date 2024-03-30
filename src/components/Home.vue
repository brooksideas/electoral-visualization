<script>
import { ref, onMounted, onBeforeUnmount } from "vue";
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
    const isSmallScreen = ref(window.innerWidth < 1400);

    const handleResize = () => {
      isSmallScreen.value = window.innerWidth < 1400;
    };

    onMounted(() => {
      window.addEventListener("resize", handleResize);
    });

    onBeforeUnmount(() => {
      window.removeEventListener("resize", handleResize);
    });

    return {
      description,
      isSmallScreen,
    };
  },
};
</script>

<template>
  <div id="app">
    <div class="grid grid-cols-12 gap-4 content-start items-center">
      <div
        class="flex justify-center w-full"
        :class="{ 'col-span-1': !isSmallScreen, 'col-span-12 mt-32': isSmallScreen }"
      >
        <display-options-list />
      </div>
      <div
        :class="{ 'col-span-8': !isSmallScreen, 'col-span-12': isSmallScreen }"
      >
        <h1>{{ description }}</h1>
        <responsive-state-map />
        <!-- <responsive-city-map /> -->
      </div>
      <div
        class="flex justify-center ml-12"
        :class="{ 'col-span-3': !isSmallScreen, 'col-span-12': isSmallScreen }"
      >
        <display-selection-list />
      </div>
    </div>
  </div>
</template>

<style>
#app {
  max-width: 1400px !important;
}
</style>