<template>
  <div id="app">
    <h1>Intervention Showcase Gallery</h1>
    <FilterBar @filter="applyFilter" />
    <div class="content">
      <InterventionList :nodes="filteredNodes" />
      <NetworkGraph :nodes="filteredNodes" :links="filteredLinks" />
    </div>
  </div>
</template>

<script>
import interventions from "@/assets/interventions.json";
import InterventionList from "@/components/InterventionList.vue";
import NetworkGraph from "@/components/NetworkGraph.vue";
import FilterBar from "@/components/FilterBar.vue";

export default {
  components: {
    InterventionList,
    NetworkGraph,
    FilterBar,
  },
  data() {
    return {
      nodes: interventions.nodes,
      links: interventions.links, // Initialize the links
      filteredNodes: interventions.nodes,
      filteredLinks: interventions.links, // Initialize filteredLinks
    };
  },
  methods: {
    applyFilter({ searchTerm, filter }) {
      this.filteredNodes = this.nodes.filter((node) => {
        const matchesSearch = node.name
          .toLowerCase()
          .includes(searchTerm.toLowerCase());

        // Determine which node property to filter on based on the selected filter
        let matchesFilter = true;
        if (filter) {
          if (filter === "cost") {
            matchesFilter =
              node.cost && node.cost.toLowerCase() === filter.toLowerCase();
          } else if (filter === "space") {
            matchesFilter =
              node.space && node.space.toLowerCase() === filter.toLowerCase();
          } else if (filter === "environmental") {
            matchesFilter =
              node.environmental &&
              node.environmental.toLowerCase() === filter.toLowerCase();
          }
        }

        return matchesSearch && matchesFilter;
      });

      // Filter links to show only those between filtered nodes
      const filteredNodeIds = new Set(
        this.filteredNodes.map((node) => node.id)
      );
      this.filteredLinks = this.links.filter(
        (link) =>
          filteredNodeIds.has(link.source) && filteredNodeIds.has(link.target)
      );
    },
  },
};
</script>

<style>
#app {
  font-family: "Roboto", sans-serif;
  color: #2c3e50;
  padding: 20px;
  background-color: #f5f7fa;
}

h1 {
  font-size: 2.5em;
  color: #2d6187;
  margin-bottom: 20px;
}

.content {
  display: flex;
  flex-direction: column;
  gap: 20px;
  align-items: center;
  display: block;
}
</style>
