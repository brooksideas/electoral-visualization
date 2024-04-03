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
      labels: [1980, 1985, 1990, 1995, 2000, 2005, 2010, 2015, 2020],
      datasets: [
        {
          label: "NY and CT",
          data: [50, 78, 23, 34, 89, 32, 67, 44, 56],
          backgroundColor: "#3b82f6",
        },
        {
          label: "ALL Other States",
          data: [
            100 - 50,
            100 - 78,
            100 - 23,
            100 - 34,
            100 - 89,
            100 - 32,
            100 - 67,
            100 - 44,
            100 - 56,
          ],
          backgroundColor: "#ef4444",
        },
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