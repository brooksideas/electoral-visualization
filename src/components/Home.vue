<script>
import { inject, ref, onMounted, onBeforeUnmount, computed } from "vue";
import { views } from "../constants/views";
import DisplayOptionsList from "./DisplayOptionsList.vue";
import ResponsiveHouseMap from "./ResponsiveHouseMap.vue";
import ResponsiveFundingMap from "./ResponsiveFundingMap.vue";
import ResponsiveChart from "./ResponsiveChart.vue";
import DisplaySelectionList from "./DisplaySelectionList.vue";
export default {
  name: "Home",
  components: {
    DisplayOptionsList,
    ResponsiveHouseMap,
    ResponsiveFundingMap,
    ResponsiveChart,
    DisplaySelectionList,
  },
  setup() {
    // Inject the Event Bus
    const bus = inject("$bus");

    // Display view
    const displayView = ref(views.HOUSE);

    // Listen for view render Event
    bus.on("renderEvt", (renderEvt) => {
      displayView.value = renderEvt;
    });

    const title = ref("Electoral Visualization");
    const description = ref("House Data Visualization");
    const isSmallScreen = ref(window.innerWidth < 1400);

    const handleResize = () => {
      isSmallScreen.value = window.innerWidth < 1400;
    };

    const handleDescriptionRender = computed(() => {
      switch (displayView.value) {
        case views.HOUSE:
          return "House Data Visualization";
        case views.FUNDING:
          return "Funding Data Visualization";
        case views.CHARTS:
          return "Total Votes count Visualization";
      }
    });

    onMounted(() => {
      window.addEventListener("resize", handleResize);
    });

    onBeforeUnmount(() => {
      window.removeEventListener("resize", handleResize);
    });

    return {
      views,
      title,
      description,
      displayView,
      isSmallScreen,
      handleDescriptionRender,
    };
  },
};
</script>

<template>
  <div id="app">
    <div class="grid grid-cols-12 gap-4 content-start items-center">
      <div
        class="flex justify-center w-full"
        :class="{
          'col-span-1': !isSmallScreen,
          'col-span-12 mt-32': isSmallScreen,
        }"
      >
        <display-options-list />
      </div>
      <div
        :class="{ 'col-span-8': !isSmallScreen, 'col-span-12': isSmallScreen }"
      >
        <h1>{{ title }}</h1>
        <h2>{{ handleDescriptionRender }}</h2>
        <responsive-house-map v-if="displayView == views.HOUSE" />
        <responsive-funding-map v-if="displayView == views.FUNDING" />
        <responsive-chart v-if="displayView == views.CHARTS" />
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