<template>
    <div>
      <h2>Interventions</h2>
      <input v-model="searchQuery" placeholder="Search interventions..." class="search-bar" />
      <div class="card-grid">
        <div class="card" v-for="node in filteredNodes" :key="node.id">
          <h3>{{ node.name }}</h3>
          <p>Learn more about this intervention.</p>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      nodes: Array
    },
    data() {
      return {
        searchQuery: ""
      };
    },
    computed: {
      filteredNodes() {
        return this.nodes.filter(node =>
          node.name.toLowerCase().includes(this.searchQuery.toLowerCase())
        );
      }
    }
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
    grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
    gap: 20px;
    margin-top: 20px;
  }
  
  .card {
    background-color: white;
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    transition: transform 0.3s, box-shadow 0.3s;
  }
  
  .card:hover {
    transform: translateY(-5px);
    box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
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
  </style>
  