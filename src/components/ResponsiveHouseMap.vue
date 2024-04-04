<template>
  <div id="d3-container"></div>
</template>

<script>
import * as d3 from "d3";
import * as topojson from "topojson-client";
import data from "../data/data.json";
import ny from "../data/ny.json"; // assumed to be fetched when integrated
import az from "../data/az.json"; // assumed to be fetched when integrated
import us from "../data/us.json";
import { ref, onMounted, watch } from "vue";

export default {
  setup() {
    // Map width and height
    const width = ref(1000);
    const height = ref(610);

    // point color
    const pointColor = ref("blue");

    // tooltip color as passed from event bus
    const tooltipColor = ref("#14b8a6");

    // text color
    const textColor = ref("red");

    // change
    const watchTrigger = ref(0);

    /* This is for the frontend to handle the edge cities 
    not to overlap witht the selection dropdown and pills  */
    const edgeCities = [
      "Annapolis",
      "Albany",
      "Augusta",
      "Boston",
      "Concord",
      "Dover",
      "Hartford",
      "Montpelier",
      "Providence",
      "Trenton",
    ];

    // Reactive Data
    const mergedData = ref([]);

    // Use setTimeout to introduce a delay
    // To be replaced by the Axios data response
    // or Dropdown / party button trigger
    setTimeout(() => {
      watchTrigger.value++;
    }, 5000);

    // Hoisting so Mounted lifecycle first
    onMounted(() => {
      // Load the data
      const nyData = ny;

      // Merge the data based on state name
      mergedData.value = data.map((state) => {
        const nyEntry = nyData.find((entry) => entry.state_po === state.code);
        if (nyEntry) {
          return { ...state, ...nyEntry };
        } else {
          return state;
        }
      });
      console.log("mount triggered", mergedData.value);
      // draw the map
      drawVisualization();
    });

    //TODO:: This is a test,  Watch for changes in mergedData and redraw the visualization
    watch(watchTrigger, () => {
      // Load the AZ data
      const azData = az;

      // Merge the data based on state name
      mergedData.value = data.map((state) => {
        const azEntry = azData.find((entry) => entry.state_po === state.code);
        if (azEntry) {
          return { ...state, ...azEntry };
        } else {
          return state;
        }
      });

      console.log("watch triggered", mergedData.value);

      // Assuming we only need to update for changed data
      updateVisualization(mergedData.value);
    });

    const drawVisualization = () => {
      const svg = d3
        .select("#d3-container")
        .append("svg")
        .attr("width", width.value)
        .attr("height", height.value);

      const path = d3.geoPath();

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

      updateVisualization(mergedData.value);
    };

    const updateVisualization = (data) => {
      const svg = d3.select("#d3-container").select("svg"); // select the map to re-draw

      const projection = d3
        .geoAlbersUsa()
        .translate([width.value / 2, height.value / 2]) // Translate to the center of SVG
        .scale([1300]); // Scale the projection to 1300 for my screen

      const stateCapitalElements = svg
        .selectAll("g")
        .data(data, (d) => d.state) // Update based on state name
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
        .attr("font-size", (d) => (edgeCities.includes(d.city) ? 12 : 15)) // 10 for edge states
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
    };

    return {
      mergedData,
    };
  },
};
</script>

<style scoped>
#d3-container {
  width: 100%;
  height: 100%;
}
</style>
