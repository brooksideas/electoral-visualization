<template>
  <div id="d3-container"></div>
</template>
<script>
import * as d3 from "d3";
import * as topojson from "topojson-client";
import data from "../data/data.json";
import us from "../data/us.json";
import { ref, onMounted } from "vue";
export default {
  setup() {
    // point color
    const pointColor = ref("blue");

    // tooltip color as passed from event bus
    const tooltipColor = ref("black");

    // text color
    const textColor = ref("red");

    onMounted(() => {
      const width = 975;
      const height = 610;

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

      const stateCapitalElements = svg.selectAll("g").data(data).join("g");

      const capitalGroups = stateCapitalElements
        .append("g")
        .attr(
          "transform",
          ({ longitude, latitude }) =>
            `translate(${projection([longitude, latitude]).join(",")})`
        )
        .on("mouseover", function (evt, d) {
          d3.select(this).select("text.tooltip").style("visibility", "visible");
        })
        .on("mouseout", function (evt, d) {
          d3.select(this).select("text.tooltip").style("visibility", "hidden");
        });

      capitalGroups
        .append("circle")
        .attr("r", 4)
        .attr("fill", `${pointColor.value}`); // Add fill color for circle dots

      // Let's create a tooltip SVG text element
      const tooltip = capitalGroups
        .append("text")
        .attr("class", "tooltip")
        .attr("fill", "black")
        .style("visibility", "hidden") // Initially hide the tooltip
        .attr("fill", `${tooltipColor.value}`)
        .style("background-color", "#fff") // Set background color inline
        .style("border", "1px solid #ccc") // Set border inline
        .style("padding", "5px") // Set padding inline
        .style("font-weight", "bold"); // Set font weight inline

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
          const tooltipText = `${d.description[0]}`;
          tooltip
            .selectAll("tspan")
            .data(tooltipText.split("\n"))
            .join("tspan")
            .attr("dy", "1em")
            .attr("x", "0px")
            .text((text) => text);
        })
        .on("mouseout", function (evt, d) {
          d3.select(this).transition().duration("200").attr("font-size", 11);
          tooltip.selectAll("tspan").remove();
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
