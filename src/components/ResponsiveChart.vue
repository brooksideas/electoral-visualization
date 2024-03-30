<template>
  <Bar id="third-chart-id" :options="chartOptions" :data="chartData" />
</template>
  
  <script>
import { reactive } from "vue";
import { Bar } from "vue-chartjs";
import {
  Chart as ChartJS,
  Title,
  Tooltip,
  Legend,
  BarElement,
  CategoryScale,
  LinearScale,
} from "chart.js";

ChartJS.register(
  Title,
  Tooltip,
  Legend,
  BarElement,
  CategoryScale,
  LinearScale
);

export default {
  name: "ResponsiveChart",
  components: { Bar },
  setup() {
    const chartData = reactive({
      labels: ["January", "February", "March"],
      datasets: [
        { label: "Months", data: [40, 20, 12], backgroundColor: "#14b8a6" },
      ],
    });
    const chartOptions = reactive({
      responsive: true,
      plugins: {
        legend: {
          position: "right",
        },
        title: {
          display: true,
          text: "Third Party Chart",
        },
      },
    });

    return { chartData, chartOptions };
  },
};
</script>