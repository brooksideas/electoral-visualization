<template>
  <div id="d3-container"></div>
</template>
  <script>
import * as d3 from "d3";
import * as topojson from "topojson-client";
import city from "../data/city.json";
import cities from "../data/cities.json";
import us from "../data/us.json";
import { onMounted } from "vue";
export default {
  setup() {
    onMounted(() => {
      const width = 975;
      const height = 610;
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
        .scale([1300]); // Scale the projection to 1300 for my screen

      // Assuming 'us' and 'cities' are defined elsewhere
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
    
    
      const citiesSubset = cities.slice(0, 100); // Fetching the first 100 records

      const stateCapitalElements = svg
        .selectAll("g")
        .data(citiesSubset)
        .join("g");

      // Handle Null cases for missing cities
      const capitalGroups = stateCapitalElements
        .append("g")
        .attr("transform", ({ longitude, latitude }) => {
          const projectedCoordinates = projection([longitude, latitude]);
          if (projectedCoordinates) {
            return `translate(${projectedCoordinates.join(",")})`;
          } else {
            // Handle missing or invalid coordinates
            return ""; // or any default transformation
          }
        });

      capitalGroups.append("circle").attr("r", 2).attr("fill", "red"); // Add fill color for circle dots

      // Adjust the text positioning
      capitalGroups
        .append("text")
        .attr("font-family", "sans-serif")
        .attr("font-size", 11)
        .attr("text-anchor", "middle")
        .attr("fill", "red")
        .on("mouseover", function (d, i) {
          d3.select(this).transition().duration("100").attr("font-size", 18);
        })
        .on("mouseout", function (d, i) {
          d3.select(this).transition().duration("200").attr("font-size", 11);
        })
        .style("cursor", "pointer") // Set cursor style to pointer
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
  