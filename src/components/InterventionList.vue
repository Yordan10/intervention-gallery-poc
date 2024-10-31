<template>
  <div>
    <h2>Interventions</h2>
    <input
      v-model="searchQuery"
      placeholder="Search interventions..."
      class="search-bar"
    />
    <div v-if="filteredNodes.length === 0" class="no-results">
      No results found.
    </div>
    <div v-else class="card-grid">
      <div
        class="card"
        v-for="node in filteredNodes"
        :key="node.id"
        @click="selectNode(node)"
      >
        <img
          :src="node.image || defaultImage"
          alt="Intervention Image"
          class="card-image"
        />
        <h3>{{ node.name }}</h3>
        <p>{{ node.description }}</p>
      </div>
    </div>

    <!-- Modal to show more details about the selected intervention -->
    <div v-if="selectedNode" class="modal">
      <div class="modal-content">
        <span class="close" @click="clearSelection">&times;</span>
        <h3>{{ selectedNode.name }}</h3>
        <img
          :src="selectedNode.image || defaultImage"
          alt="Intervention Image"
          class="modal-image"
        />
        <p>{{ selectedNode.description }}</p>
        <p>{{ selectedNode.details }}</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    nodes: Array,
  },
  data() {
    return {
      searchQuery: "",
      selectedNode: null,
      // Default image or icon in case intervention image is missing
      defaultImage: "https://via.placeholder.com/150?text=No+Image",
    };
  },
  computed: {
    filteredNodes() {
      const query = this.searchQuery.toLowerCase();
      const result = this.nodes.filter((node) =>
        node.name.toLowerCase().includes(query)
      );

      return result;
    },
  },
  methods: {
    selectNode(node) {
      this.selectedNode = node;
    },
    clearSelection() {
      this.selectedNode = null;
    },
  },

};
</script>

<style scoped>
.search-bar {
  margin-bottom: 20px;
  padding: 10px;
  width: 100%;
  border: 1px solid #ddd;
  border-radius: 5px;
}

.card-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
  gap: 20px;
  margin-top: 20px;
}

.card {
  background-color: white;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s, box-shadow 0.3s;
  cursor: pointer;
}

.card:hover {
  transform: translateY(-5px);
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
}

.card-image {
  width: 100%;
  height: 150px;
  object-fit: contain; /* Ensure the entire image is visible */
  border-radius: 5px;
  margin-bottom: 10px;
}

.card h3 {
  font-size: 1.2em;
  color: #2d6187;
  margin-bottom: 10px;
}

.card p {
  font-size: 0.9em;
  color: #555;
}

.modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1;
}

.modal-content {
  background-color: white;
  padding: 20px;
  border-radius: 10px;
  width: 80%;
  max-width: 600px;
  position: relative;
}

.close {
  position: absolute;
  top: 10px;
  right: 10px;
  font-size: 1.5em;
  cursor: pointer;
}

.modal-image {
  width: 100%;
  height: 300px;
  object-fit: contain; /* Ensure the entire image is visible */
  border-radius: 5px;
  margin-bottom: 10px;
}
</style>
