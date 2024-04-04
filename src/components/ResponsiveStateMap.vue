<template>
  <div id="d3-container"></div>
</template>

<script>
import * as d3 from "d3";
import * as topojson from "topojson-client";
import data from "../data/data.json";
import ny from "../data/ny.json";
import us from "../data/us.json";
import { ref, onMounted } from "vue";

export default {
  setup() {
    // point color
    const pointColor = ref("blue");

    // tooltip color as passed from event bus
    const tooltipColor = ref("#14b8a6");

    // text color
    const textColor = ref("red");

    /* This is for the frontend to handle the edge cities 
    not to overlap witht the selection dropdown and pills  */
    const edgeCities = [
      "Annapolis",
      "Augusta",
      "Boston",
      "Concord",
      "Dover",
      "Hartford",
      "Montpelier",
      "Providence",
      "Trenton",
    ];

    onMounted(() => {
      const width = 1000;
      const height = 610;

      // Load the data
      const nyData = ny;

      // Merge the data based on state name
      const mergedData = data.map((state) => {
        const nyEntry = nyData.find((entry) => entry.state === state.state);
        if (nyEntry) {
          const t = { ...state, ...nyEntry };
          return { ...state, ...nyEntry };
        } else {
          return state;
        }
      });

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

      const stateCapitalElements = svg
        .selectAll("g")
        .data(mergedData)
        .join("g");

      const capitalGroups = stateCapitalElements
        .append("g")
        .attr(
          "transform",
          ({ longitude, latitude }) =>
            `translate(${projection([longitude, latitude]).join(",")})`
        )
        .on("mouseover", function (evt, d) {
          d3.select(this).select(".tooltip").style("visibility", "visible");
        })
        .on("mouseout", function (evt, d) {
          d3.select(this).select(".tooltip").style("visibility", "hidden");
        });

      capitalGroups
        .append("circle")
        .attr("r", 4)
        .attr("fill", `${pointColor.value}`); // Add fill color for circle dots

      // Let's create a tooltip div container
      const tooltip = capitalGroups
        .append("foreignObject")
        .attr("class", "tooltip")
        .attr("width", (d) => (edgeCities.includes(d.city) ? 80 : 150)) // 80 for edge states
        .attr("height", 150)
        .attr("font-size", (d) => (edgeCities.includes(d.city) ? 10 : 15)) // 10 for edge states
        .attr("font-weight", "bold")
        .style("visibility", "hidden");

      tooltip
        .append("xhtml:div")
        .attr("class", "tooltip-content")
        .style("background-color", `${tooltipColor.value}`)
        .style("color", "#fff")
        .style("border", "2px solid white")
        .style("padding", "5px")
        .style("border-radius", "5px")
        .style("pointer-events", "none")
        .html((d) => `Votes: ${d.candidatevotes} \n Total: ${d.totalvotes}`);

      // Adjust the text positioning
      capitalGroups
        .append("text")
        .attr("font-family", "sans-serif")
        .attr("font-size", 11)
        .attr("text-anchor", "middle")
        .attr("fill", `${textColor.value}`)
        // Change the contents and position of the tooltip
        .on("mouseover", function (evt, d) {
          d3.select(this).transition().duration("100").attr("font-size", 18);
          d3.select(this).select(".tooltip").style("visibility", "visible");
          // Reduce opacity of other labels
          capitalGroups
            .selectAll("text")
            .filter((_, i) => i !== d.index) // Exclude the hovered label
            .transition()
            .duration("100")
            .style("opacity", 0.5);

          // Reduce opacity of other points
          capitalGroups
            .selectAll("circle")
            .filter((_, i) => i !== d.index) // Exclude the hovered point
            .transition()
            .duration("100")
            .style("opacity", 0.5);
        })
        .on("mouseout", function (evt, d) {
          d3.select(this).transition().duration("200").attr("font-size", 11);
          d3.select(this).select(".tooltip").style("visibility", "hidden");
          // Restore opacity of other labels
          capitalGroups
            .selectAll("text")
            .transition()
            .duration("200")
            .style("opacity", 1);

          // Restore opacity of other points
          capitalGroups
            .selectAll("circle")
            .transition()
            .duration("200")
            .style("opacity", 1);
        })
        .style("cursor", "pointer") // Set cursor style to pointer
        .text(({ city }) => city);
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
