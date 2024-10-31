<template>
  <div style="position: relative">
    <div ref="graph" class="graph-container"></div>
    <div v-if="selectedNode" class="info-panel">
      <h3>
        {{ selectedNode.label || selectedNode.name || "No Node Selected" }}
      </h3>
      <p>More details about the intervention could go here.</p>
      <p>Related Interventions:</p>
      <ul>
        <li v-for="related in relatedNodes" :key="related.id">
          {{ related.label || related.name }}
        </li>
      </ul>
      <button @click="clearSelection">Close</button>
    </div>
  </div>
</template>

<script>
import * as d3 from "d3";

export default {
  props: {
    nodes: Array,
    links: Array,
  },
  data() {
    return {
      selectedNode: null,
      relatedNodes: [],
    };
  },
  mounted() {
    this.initD3();
  },
  watch: {
    nodes: "initD3",
    links: "initD3",
  },
  methods: {
    initD3() {
      d3.select(this.$refs.graph).select("svg").remove(); // Clear previous SVG

      const nodes = this.nodes || []; // Fallback if nodes is undefined
      const links = this.links || []; // Fallback if links is undefined

      if (links.length === 0) {
        console.warn("No links available to display.");
      }

      const width =
        this.$refs.graph.clientWidth > 50 ? this.$refs.graph.clientWidth : 800;
      const height =
        this.$refs.graph.clientHeight > 50
          ? this.$refs.graph.clientHeight
          : 600;

      const svg = d3
        .select(this.$refs.graph)
        .append("svg")
        .attr("width", width)
        .attr("height", height)
        .style("border", "1px solid #ddd");

      const simulation = d3
        .forceSimulation(nodes)
        .force(
          "link",
          d3
            .forceLink(links)
            .id((d) => d.id)
            .distance(100)
        )
        .force("charge", d3.forceManyBody().strength(-300))
        .force("center", d3.forceCenter(width / 2, height / 2))
        .force("collide", d3.forceCollide().radius(50));

      const link = svg
        .append("g")
        .attr("class", "links")
        .selectAll("line")
        .data(links)
        .enter()
        .append("line")
        .attr("stroke-width", 2)
        .attr("stroke", "#9ecae1");

      const node = svg
        .append("g")
        .attr("class", "nodes")
        .selectAll("circle")
        .data(nodes)
        .enter()
        .append("circle")
        .attr("r", 15)
        .attr("fill", "#2d6187")
        .call(
          d3
            .drag()
            .on("start", dragstarted)
            .on("drag", dragged)
            .on("end", dragended)
        )
        .on("click", (event, d) => this.selectNode(d));

      node.append("title").text((d) => d.name);

      const label = svg
        .append("g")
        .attr("class", "labels")
        .selectAll("text")
        .data(nodes)
        .enter()
        .append("text")
        .attr("dy", -20)
        .attr("text-anchor", "middle")
        .attr("font-size", 12)
        .attr("fill", "#333")
        .text((d) => d.name);

      simulation.on("tick", () => {
        link
          .attr("x1", (d) => d.source.x)
          .attr("y1", (d) => d.source.y)
          .attr("x2", (d) => d.target.x)
          .attr("y2", (d) => d.target.y);

        node.attr("cx", (d) => d.x).attr("cy", (d) => d.y);

        label.attr("x", (d) => d.x).attr("y", (d) => d.y);
      });

      function dragstarted(event, d) {
        if (!event.active) simulation.alphaTarget(0.3).restart();
        d.fx = d.x;
        d.fy = d.y;
      }

      function dragged(event, d) {
        d.fx = event.x;
        d.fy = event.y;
      }

      function dragended(event, d) {
        if (!event.active) simulation.alphaTarget(0);
        d.fx = null;
        d.fy = null;
      }
    },
    selectNode(node) {
      this.selectedNode = node;
      this.relatedNodes = this.links
        .filter(
          (link) => link.source.id === node.id || link.target.id === node.id
        )
        .map((link) =>
          link.source.id === node.id ? link.target : link.source
        );
    },
    clearSelection() {
      this.selectedNode = null;
      this.relatedNodes = [];
    },
  },
};
</script>

<style scoped>
.graph-container {
  width: 100%;
  height: 500px;
  border: 1px solid #ddd;
  border-radius: 8px;
  margin-top: 20px;
  background-color: #fafafa;
}

.info-panel {
  position: absolute;
  top: 20px;
  right: 20px;
  width: 250px;
  max-height: 300px;
  overflow-y: auto;
  padding: 20px;
  background-color: white;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  font-size: 14px;
}

.info-panel h3 {
  font-size: 1.2em;
  color: #2d6187;
  margin-bottom: 10px;
}

.info-panel ul {
  padding-left: 15px;
}

.info-panel button {
  margin-top: 10px;
  padding: 5px 10px;
  background-color: #2d6187;
  color: #fff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.info-panel button:hover {
  background-color: #4a90a6;
}
</style>
