<template>
  <!-- This is for the House Data Party selection   -->
  <!-- <div class="grid grid-cols-1 gap-4 ml-12">
    <div v-for="i in 10" :key="i">
      <button
        type="button"
        class="w-4 h-4 rounded-full"
        :style="{ 'background-color': getRandomColorCombination() }"
      ></button>
    </div>
  </div> -->

  <!-- This is for the Contribution Data -->
  <!-- <div class="grid grid-cols-3 gap-4 ml-16">
    <div v-for="year in years" :key="year">
      <button
        type="button"
        class="inline-block px-3 py-2 gap-4 text-white font-semibold rounded-md shadow-md bg-slate-500 hover:bg-slate-600 focus:outline-none focus:ring-2 focus:ring-slate-400 focus:ring-opacity-50"
      >
        {{ year }}
      </button>
    </div>
  </div> -->

  <!-- <div class="grid grid-cols-1 gap-4 ml-12 mt-[-64px]">
    <dropdown-options-list />
  </div> -->

  <div class="grid grid-cols-1 gap-4 ml-12 mt-[-64px]">
    <slider-options-list />
  </div>
</template>
  
  <script>
import { ref, computed } from "vue";
import { colors } from "../constants/colors";
import DropdownOptionsList from "./DropdownOptionsList.vue";
import SliderOptionsList from "./SliderOptionsList.vue";

export default {
  name: "DisplaySelectionList",
  components: {
    DropdownOptionsList,
    SliderOptionsList
  },
  setup() {
    // List of color names
    const colorsList = Object.keys(colors);
    const shadeList = [50, 100, 200, 300, 400, 500, 600, 700, 800, 900, 950];

    // Years list
    const years = ref([2008, 2010, 2012, 2014, 2016, 2018, 2020, 2022]);

    // // Random value returned
    const getRandomFromArray = (arr) => {
      const randomIndex = Math.floor(Math.random() * arr.length);
      return arr[randomIndex];
    };

    // get Random color combination
    const getRandomColorCombination = () => {
      const randomColor = getRandomFromArray(colorsList);
      const randomShade = getRandomFromArray(shadeList);
      // Access the random color value using the random color name and shade
      const randomColorValue = colors[randomColor][randomShade];

      return `${randomColorValue}`;
    };

    return { years, getRandomColorCombination };
  },
};
</script>
  
<style scoped>
.display-party-margin {
  margin-top: 100px !important;
  margin-left: 1500px !important;
}
</style>
  