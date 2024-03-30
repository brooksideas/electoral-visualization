<template>
  <div class="relative" @mouseenter="showTooltip" @mouseleave="hideTooltip">
    <div v-show="displayTooltip" class="bg-gray-500">
      <span
        class="align-middle text-sm font-bold text-white bg-gray-500 bg-opacity-80 px-2 py-1 rounded-md tooltip"
      >
        The Transaction Amount contributed in Dollars. Please select the range.
      </span>
    </div>

    <div class="flex gap-4">
      <h3>Transaction Amount</h3>
      <svg
        class="w-6 h-6 text-gray-800 dark:text-white"
        aria-hidden="true"
        xmlns="http://www.w3.org/2000/svg"
        width="24"
        height="24"
        fill="none"
        viewBox="0 0 24 24"
      >
        <path
          stroke="currentColor"
          stroke-linecap="round"
          stroke-linejoin="round"
          stroke-width="2"
          d="M9.529 9.988a2.502 2.502 0 1 1 5 .191A2.441 2.441 0 0 1 12 12.582V14m-.01 3.008H12M21 12a9 9 0 1 1-18 0 9 9 0 0 1 18 0Z"
        />
      </svg>
    </div>
  </div>

  <div class="grid grid-cols-12 gap-4 content-start items-center">
    <div class="col-span-6">
      <input
        type="range"
        class="mr-4 cursor-pointer"
        :min="minValue"
        :max="maxValue / 2"
        :step="stepValue"
        v-model="minSliderValue"
        @input="updateMaxSliderMinValue"
      />
    </div>
    <div class="col-span-6 pl-4">
      <input
        type="range"
        class="mr-4 cursor-pointer"
        :min="maxValue / 2 + 1"
        :max="maxValue"
        :step="stepValue"
        v-model="maxSliderValue"
        @input="updateMinSliderMaxValue"
      />
    </div>

    <div class="mr-2 col-span-6">
      Min:
      <input
        id="min-slider"
        type="text"
        inputmode="numeric"
        v-model="formattedMinSliderValue"
        @input="handleMinInput"
        class="w-24 mx-2 py-1 border rounded-md"
      />
    </div>
    <div class="ml-8 col-span-6">
      Max:
      <input
        id="max-slider"
        type="text"
        inputmode="numeric"
        v-model="formattedMaxSliderValue"
        @input="handleMaxInput"
        class="w-24 mx-2 py-1 border rounded-md"
      />
    </div>

    <div class="col-span-12 ml-12">
      <button
        class="px-4 py-2 bg-blue-500 hover:bg-blue-600 text-white font-semibold rounded-md shadow-md transition duration-300 ease-in-out"
      >
        Search
      </button>
    </div>
  </div>
</template>
  
  <script>
import { ref, computed } from "vue";

export default {
  setup() {
    const minValue = 25;
    const maxValue = 2800000;
    const stepValue = 1;
    const displayTooltip = ref(false);

    const minSliderValue = ref(minValue);
    const maxSliderValue = ref(maxValue);

    const updateMaxSliderMinValue = (event) => {
      if (minSliderValue.value > maxSliderValue.value) {
        maxSliderValue.value = minSliderValue.value;
      }
    };

    const updateMinSliderMaxValue = (event) => {
      if (maxSliderValue.value < minSliderValue.value) {
        minSliderValue.value = maxSliderValue.value;
      }
    };
    const showTooltip = () => {
      displayTooltip.value = true;
    };

    const hideTooltip = () => {
      displayTooltip.value = false;
    };

    // Define a computed property to format minSliderValue
    const formattedMinSliderValue = computed({
      get() {
        return minSliderValue.value
          .toString()
          .replace(/\B(?=(\d{3})+(?!\d))/g, ",");
      },
      set(newValue) {
        // Remove commas before setting the value
        const newValueWithoutCommas = newValue.replace(/,/g, "");
        minSliderValue.value = parseInt(newValueWithoutCommas);
      },
    });

    const handleMinInput = (event) => {
      let newValue = event.target.value.replace(/\D/g, ""); // Remove non-numeric characters
      if (newValue === "" || isNaN(newValue)) {
        newValue = 0;
      }
      if (newValue > maxValue / 2) {
        minSliderValue.value = maxValue / 2;
      } else {
        minSliderValue.value = parseInt(newValue);
      }
    };

    // Handle Max sliders
    const formattedMaxSliderValue = computed({
      get() {
        return maxSliderValue.value
          .toString()
          .replace(/\B(?=(\d{3})+(?!\d))/g, ",");
      },
      set(newValue) {
        // Remove commas before setting the value
        const newValueWithoutCommas = newValue.replace(/,/g, "");
        maxSliderValue.value = parseInt(newValueWithoutCommas);
      },
    });

    const handleMaxInput = (event) => {
      let newValue = event.target.value.replace(/\D/g, ""); // Remove non-numeric characters
      if (newValue === "" || isNaN(newValue)) {
        newValue = 0;
      }
      if (newValue > maxValue) {
        maxSliderValue.value = maxValue;
      } else {
        maxSliderValue.value = parseInt(newValue);
      }
    };

    return {
      minValue,
      maxValue,
      stepValue,
      minSliderValue,
      maxSliderValue,
      showTooltip,
      hideTooltip,
      displayTooltip,
      handleMinInput,
      handleMaxInput,
      updateMaxSliderMinValue,
      updateMinSliderMaxValue,
      formattedMinSliderValue,
      formattedMaxSliderValue,
    };
  },
};
</script> 