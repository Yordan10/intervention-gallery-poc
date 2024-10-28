<template>
    <div class="filter-bar">
      <input v-model="searchTerm" placeholder="Search interventions..." />
      <button @click="applyFilter">Apply Filter</button>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        searchTerm: ""
      };
    },
    methods: {
      applyFilter() {
        this.$emit("filter", this.searchTerm);
      }
    }
  };
  </script>
  
  <style scoped>
  .filter-bar {
    display: flex;
    gap: 10px;
    margin-bottom: 20px;
    justify-content: center;
  }
  
  input {
    padding: 10px;
    border-radius: 5px;
    border: 1px solid #ddd;
    flex-grow: 1;
    max-width: 300px;
  }
  
  button {
    padding: 10px 20px;
    background-color: #2d6187;
    color: #fff;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.3s;
  }
  
  button:hover {
    background-color: #4a90a6;
  }
  </style>
  