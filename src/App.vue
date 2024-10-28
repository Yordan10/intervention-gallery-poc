<template>
  <div id="app">
    <h1>Intervention Showcase Gallery</h1>
    <FilterBar @filter="applyFilter" />
    <div class="content">
      <InterventionList :nodes="filteredNodes" />
      <NetworkGraph :nodes="nodes" :links="links" />
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
    FilterBar
  },
  data() {
    return {
      nodes: interventions.nodes,
      links: interventions.links,
      filteredNodes: interventions.nodes
    };
  },
  methods: {
    applyFilter(searchTerm) {
      this.filteredNodes = this.nodes.filter(node =>
        node.name.toLowerCase().includes(searchTerm.toLowerCase())
      );
    }
  }
};
</script>

<style>
#app {
  font-family: 'Roboto', sans-serif;
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
}
</style>
