<template>
  <div class="container">
    <svg ref="svgRef"></svg>
  </div>
</template>

<script>
import { ref, onMounted } from "vue";
import * as d3 from "d3";

export default {
  setup() {
    const svgRef = ref(null);

    onMounted(() => {
      const svg = d3.select(svgRef.value).attr("width", 400).attr("height", 400);
      const centerX = 200, centerY = 200, outerRadius = 100;

      svg.append("circle")
        .attr("cx", centerX)
        .attr("cy", centerY)
        .attr("r", outerRadius)
        .attr("fill", "#2E2E2E");

      svg.append("circle")
        .attr("cx", centerX)
        .attr("cy", centerY)
        .attr("r", 80)
        .attr("fill", "white")
        .attr("stroke", "gray")
        .attr("stroke-width", 3);

      const conductors = [
        { x: centerX - 29, y: centerY - 35, color: "orange" },
        { x: centerX + 14, y: centerY - 35, color: "#FFE4E1" },
        { x: centerX - 25, y: centerY + 40, color: "blue" },
        { x: centerX + 17, y: centerY + 39, color: "#FFE4E1" }
      ];

      conductors.forEach(d => {
        svg.append("circle")
          .attr("cx", d.x)
          .attr("cy", d.y)
          .attr("r", 21)
          .attr("fill", "none")
          .attr("stroke", "black")
          .attr("stroke-width", 1.5);

        svg.append("circle")
          .attr("cx", d.x)
          .attr("cy", d.y)
          .attr("r", 18)
          .attr("fill", "white")
          .attr("stroke", d.color)
          .attr("stroke-width", 5);
      });

      svg.append("ellipse")
        .attr("cx", (conductors[0].x + conductors[1].x) / 2)
        .attr("cy", (conductors[0].y + conductors[1].y) / 2 - 3)
        .attr("rx", 44)
        .attr("ry", 38)
        .attr("fill", "none")
        .attr("stroke", "gray")
        .attr("stroke-width", 1);

      svg.append("ellipse")
        .attr("cx", (conductors[2].x + conductors[3].x) / 2)
        .attr("cy", (conductors[2].y + conductors[3].y) / 2)
        .attr("rx", 44)
        .attr("ry", 38)
        .attr("fill", "none")
        .attr("stroke", "gray")
        .attr("stroke-width", 1);

      const strandRadius = 5;
      const strandColor = "#A0522D";
      const strandOffsets = [
        { dx: 0, dy: 0 },
        { dx: 10, dy: 0 }, { dx: -10, dy: 0 },
        { dx: 5, dy: 8 }, { dx: -5, dy: 8 },
        { dx: 5, dy: -8 }, { dx: -5, dy: -8 }
      ];

      conductors.forEach(d => {
        strandOffsets.forEach(offset => {
          svg.append("circle")
            .attr("cx", d.x + offset.dx)
            .attr("cy", d.y + offset.dy)
            .attr("r", strandRadius)
            .attr("fill", strandColor)
            .attr("stroke", "black")
            .attr("stroke-width", 1);
        });
      });

      const extraStarX = centerX + 60;
      const extraStarY = centerY - 50;

      strandOffsets.forEach(offset => {
        svg.append("circle")
          .attr("cx", extraStarX + offset.dx)
          .attr("cy", extraStarY + offset.dy)
          .attr("r", strandRadius)
          .attr("fill", strandColor)
          .attr("stroke", "black")
          .attr("stroke-width", 1);
      });

      const arc = d3.arc()
        .innerRadius(48)
        .outerRadius(50)
        .startAngle(-Math.PI / 3.6)
        .endAngle(Math.PI / 5.6);

      svg.append("path")
        .attr("d", arc())
        .attr("fill", "gray")
        .attr("opacity", 0.8)
        .attr("stroke", "none")
        .attr("transform", `translate(${extraStarX + 25}, ${extraStarY - 19}) rotate(240)`);
    });

    return { svgRef };
  }
};
</script>

<style scoped>
.container {
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
