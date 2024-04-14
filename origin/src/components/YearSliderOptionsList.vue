<template>
  <div>
    <input
      type="range"
      :min="minValue"
      :max="maxValue"
      :step="stepValue"
      v-model="sliderValue"
      @input="updateRangeValues"
    />
    <div class="flex justify-between">
      <span>{{ minValue }}</span>
      <span>{{ maxValue }}</span>
    </div>

    <div class="mr-2 col-span-6">
      Year:
      <input
        id="min-slider"
        type="number"
        inputmode="numeric"
        v-model="sliderValue"
        @input="updateRangeValues"
        class="w-24 mx-2 py-1 border rounded-md"
      />
    </div>
  </div>
</template>
  
  <script>
import { ref } from "vue";

export default {
  setup() {
    const minValue = 1976;
    const maxValue = 2022;
    const stepValue = 2;

    const sliderValue = ref(minValue);

    const updateRangeValues = () => {
      // check slider value when using input
      // if less than 1976 set to 1976
      if (sliderValue.value < minValue) {
        sliderValue.value = minValue;
      }
      // if greater than 2022 set to 2022
      else if (sliderValue.value > maxValue) {
        sliderValue.value = maxValue;
      }
      // if in between return the value
      else if (sliderValue.value > minValue && sliderValue.value < maxValue) {
        return sliderValue.value;
      }
    };

    return {
      minValue,
      maxValue,
      stepValue,
      sliderValue,
      updateRangeValues,
    };
  },
};
</script> 
  