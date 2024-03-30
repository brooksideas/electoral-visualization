<template>
  <div class="flex flex-col items-start space-y-4 nav-margin">
    <!-- Iterate over display options and render list items -->
    <div class="flex flex-col flex-1 overflow-y-auto">
      <nav
        class="flex-1 px-2 py-4 nav-background rounded shadow-2xl hover:shadow-xl shadow-inner focus:shadow-xl"
      >
        <a
          href="#"
          class="flex items-center px-4 py-2 text-gray-800 hover:nav-background hover:font-bold transition duration-300 shadow-md hover:shadow-xl focus:shadow-xl"
        >
          <svg
            class="h-6 w-6 mr-2"
            aria-hidden="true"
            xmlns="http://www.w3.org/2000/svg"
            width="24"
            height="24"
            fill="none"
            viewBox="0 0 24 24"
            stroke="currentColor"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="m4 12 8-8 8 8M6 10.5V19a1 1 0 0 0 1 1h3v-3a1 1 0 0 1 1-1h2a1 1 0 0 1 1 1v3h3a1 1 0 0 0 1-1v-8.5"
            />
          </svg>

          House Data
        </a>
        <a
          href="#"
          class="flex items-center px-4 py-2 mt-2 text-gray-800 hover:nav-background hover:font-bold transition duration-300 shadow-md hover:shadow-xl focus:shadow-xl"
        >
          <svg
            class="h-6 w-6 mr-2"
            aria-hidden="true"
            xmlns="http://www.w3.org/2000/svg"
            width="24"
            height="24"
            fill="none"
            viewBox="0 0 24 24"
            stroke="currentColor"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M8 17.345a4.76 4.76 0 0 0 2.558 1.618c2.274.589 4.512-.446 4.999-2.31.487-1.866-1.273-3.9-3.546-4.49-2.273-.59-4.034-2.623-3.547-4.488.486-1.865 2.724-2.899 4.998-2.31.982.236 1.87.793 2.538 1.592m-3.879 12.171V21m0-18v2.2"
            />
          </svg>

          Funding
        </a>
        <a
          href="#"
          class="flex items-center px-4 py-2 mt-2 text-gray-800 hover:nav-background hover:font-bold transition duration-300 shadow-md hover:shadow-xl focus:shadow-xl"
        >
          <svg
            class="h-6 w-6 mr-2"
            aria-hidden="true"
            xmlns="http://www.w3.org/2000/svg"
            width="24"
            height="24"
            fill="none"
            viewBox="0 0 24 24"
            stroke="currentColor"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M4 4.5V19a1 1 0 0 0 1 1h15M7 14l4-4 4 4 5-5m0 0h-3.207M20 9v3.207"
            />
          </svg>

          Charts
        </a>
      </nav>
    </div>
  </div>
</template>
  
  <script setup>
import { ref } from "vue";

// Define display options
const displayOptions = ref([
  { id: 1, name: "House", component: "responsive-state-map" },
  { id: 2, name: "City", component: "responsive-city-map" },
  // Add more display options as needed
]);

// Track the selected display option
const selectedOption = ref(null);

// Method to select an option
const selectOption = (option) => {
  selectedOption.value = option;
  // Emit an event to inform the parent component about the selected option
  emit("option-selected", option.component);
};
</script>
  
  <style>
.nav-margin {
  margin-top: -100px !important;
  margin-left: -200px !important;
}

.nav-background {
  background-color: #bfbfbf !important;
}
</style>
  