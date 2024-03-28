<template>
  <div id="d3-container"></div>
</template>
<script>
import * as d3 from "d3";
import * as topojson from "topojson-client";
import data from "../data/data.json";
import us from "../data/us.json";
import { onMounted } from "vue";
export default {
  setup() {
    onMounted(() => {
      const width = 900;
      const height = 450;
      // const margin = { top: 50, bottom: 50, left: 50, right: 50 };

      const svg = d3
        .select("#d3-container")
        .append("svg")
        .attr("width", width)
        .attr("height", height);

      const path = d3.geoPath();

      // Define the projection
      const projection = d3
        .geoAlbersUsa()
        .translate([width / 2, height / 2]) // Translate to the center of SVG
        .scale([1000]); // Scale the projection

      // Assuming 'us' and 'data' are defined elsewhere
      svg
        .append("path")
        .datum(topojson.feature(us, us.objects.nation))
        .attr("fill", "#ddd")
        .attr("d", path);

      svg
        .append("path")
        .datum(topojson.mesh(us, us.objects.states, (a, b) => a !== b))
        .attr("fill", "none")
        .attr("stroke", "#fff")
        .attr("stroke-linejoin", "round")
        .attr("stroke-linecap", "round")
        .attr("d", path);

      const stateCapitalElements = svg.selectAll("g").data(data).join("g");

      const capitalGroups = stateCapitalElements
        .append("g")
        .attr(
          "transform",
          ({ longitude, latitude }) =>
            `translate(${projection([longitude, latitude]).join(",")})`
        );

      capitalGroups.append("circle").attr("r", 2).attr("fill", "red"); // Add fill color for circle dots
      console.log("descr->", data);
      // Adjust the text positioning
      capitalGroups
        .append("text")
        .attr("font-family", "sans-serif")
        .attr("font-size", 10)
        .attr("text-anchor", "middle")
        .attr("fill", "red")
        .text(({ description }) => description);
    });
  },
};
</script>

<style scoped>
#d3-container {
  width: 100%;
  height: 100%;
}
</style>
