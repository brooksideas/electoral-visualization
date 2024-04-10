<template>
  <!-- Page 1: House Data Party selection -->
  <div v-if="displayView === views.HOUSE" class="grid grid-cols-12 gap-4 ml-12">
    <div class="col-span-12 gap-4 ml-12">
      <dropdown-options-list />
    </div>
    <div class="col-span-8">
      <div
        v-for="(party, index) in paginatedPartyList"
        :key="index"
        class="w-full my-2"
      >
        <button
          type="button"
          class="w-4 h-4 rounded-full overflow-visible whitespace-nowrap relative"
          :style="{ 'background-color': getRandomColorCombination() }"
          @click="partySelected(party)"
        >
          <span class="absolute top-[-2px] left-[48px]">{{ party }}</span>
        </button>
      </div>
    </div>
    <div class="col-span-12 flex justify-end pl-12">
      <button
        @click="previousPage"
        :disabled="currentPage === 0"
        class="flex-1 inline-block px-3 py-2 mx-0 my-4 gap-4 text-white font-semibold rounded-md shadow-md focus:outline-none focus:ring-2 focus:ring-slate-400 focus:ring-opacity-50"
        :class="{
          'bg-slate-500 hover:bg-slate-600': currentPage !== 0,
          'bg-zinc-300 hover:bg-zinc-400': currentPage === 0,
        }"
      >
        Previous
      </button>
      <button
        @click="nextPage"
        :disabled="currentPage === totalPages - 1"
        class="flex-1 inline-block px-3 py-2 mx-2 my-4 gap-4 text-white font-semibold rounded-md shadow-md focus:outline-none focus:ring-2 focus:ring-slate-400 focus:ring-opacity-50"
        :class="{
          'bg-slate-500 hover:bg-slate-600': currentPage !== totalPages - 1,
          'bg-zinc-300 hover:bg-zinc-400': currentPage === totalPages - 1,
        }"
      >
        Next
      </button>
    </div>
  </div>

  <!-- Page 2 ,  This is for the Contribution Data -->
  <div v-if="displayView == views.FUNDING" class="grid grid-cols-12">
    <div class="col-span-12 gap-4 ml-12 mb-12">
      <div class="grid grid-cols-12">
        <div class="col-span-4" v-for="year in years" :key="year">
          <button
            type="button"
            class="inline-block px-3 py-2 mx-4 my-4 gap-4 text-white font-semibold rounded-md shadow-md bg-slate-500 hover:bg-slate-600 focus:outline-none focus:ring-2 focus:ring-slate-400 focus:ring-opacity-50"
          >
            {{ year }}
          </button>
        </div>
      </div>
    </div>

    <div class="col-span-12 gap-4 ml-12">
      <slider-options-list />
    </div>
  </div>

  <!-- Page 3 , Slider for candidate / total votes count  -->
  <div
    v-if="displayView == views.CHARTS"
    class="grid grid-cols-1 gap-4 ml-12 mt-[-64px]"
  >
    <year-slider-options-list />
    <div v-for="average in averages" :key="average" class="flex">
      <button
        type="button"
        class="inline-block w-[300px] px-3 py-2 gap-4 text-white font-semibold rounded-md shadow-md bg-slate-500 hover:bg-slate-600 focus:outline-none focus:ring-2 focus:ring-slate-400 focus:ring-opacity-50"
      >
        {{ average }}
      </button>
    </div>
  </div>
</template>

<script>
import { inject, ref, computed } from "vue";
import { views } from "../constants/views";
import { colors } from "../constants/colors";
import { partyList } from "../constants/parties";
import DropdownOptionsList from "./DropdownOptionsList.vue";
import SliderOptionsList from "./SliderOptionsList.vue";
import YearSliderOptionsList from "./YearSliderOptionsList.vue";

export default {
  name: "DisplaySelectionList",
  components: {
    DropdownOptionsList,
    SliderOptionsList,
    YearSliderOptionsList,
  },
  setup() {
    const bus = inject("$bus");

    // Listen for view render Event
    bus.on("renderEvt", (renderEvt) => {
      displayView.value = renderEvt;
    });

    const displayView = ref(views.HOUSE);

    const years = ref([2008, 2010, 2012, 2014, 2016, 2018, 2020, 2022]);

    // Average Count display
    const averages = ref(["Candidate count", "Total votes count"]);
    const itemsPerPage = 10;
    const currentPage = ref(0);

    const totalPages = computed(() =>
      Math.ceil(partyList.length / itemsPerPage)
    );
    const startIndex = computed(() => currentPage.value * itemsPerPage);
    const endIndex = computed(() => startIndex.value + itemsPerPage);
    const paginatedPartyList = computed(() =>
      partyList.slice(startIndex.value, endIndex.value)
    );

    const getRandomFromArray = (arr) => {
      const randomIndex = Math.floor(Math.random() * arr.length);
      return arr[randomIndex];
    };

    const getRandomColorCombination = () => {
      const colorsList = Object.keys(colors);
      const shadeList = [50, 100, 200, 300, 400, 500, 600, 700, 800, 900, 950];
      const randomColor = getRandomFromArray(colorsList);
      const randomShade = getRandomFromArray(shadeList);
      const randomColorValue = colors[randomColor][randomShade];
      return `${randomColorValue}`;
    };

    const partySelected = (party) => {
      bus.emit("partySelectionEvt", party);
    };

    const nextPage = () => {
      if (currentPage.value < totalPages.value - 1) {
        currentPage.value++;
      }
    };

    const previousPage = () => {
      if (currentPage.value > 0) {
        currentPage.value--;
      }
    };

    return {
      views,
      partyList,
      years,
      averages,
      displayView,
      paginatedPartyList,
      getRandomColorCombination,
      partySelected,
      nextPage,
      previousPage,
      totalPages,
      currentPage,
    };
  },
};
</script>

<style scoped>
.display-party-margin {
  margin-top: 100px !important;
  margin-left: 1500px !important;
}
.overflow-visible {
  overflow: visible;
}
</style>
