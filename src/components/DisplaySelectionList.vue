<template>
  <!-- This is for the House Data Party selection   -->
  <div class="grid grid-cols-1 gap-4 ml-12">
    <div v-for="i in 10" :key="i">
      <button
        type="button"
        class="w-4 h-4 rounded-full"
        :style="{ 'background-color': getRandomColorCombination() }"
      ></button>
    </div>
  </div>
</template>
  
  <script>
import { ref, computed } from "vue";
import { colors } from "../constants/colors";
export default {
  name: "DisplaySelectionList",
  setup() {
    // List of color names
    const colorsList = Object.keys(colors);
    const shadeList = [50, 100, 200, 300, 400, 500, 600, 700, 800, 900, 950];

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

    return { getRandomColorCombination };
  },
};
</script>
  
<style scoped>
.display-party-margin {
  margin-top: 100px !important;
  margin-left: 1500px !important;
}
</style>
  