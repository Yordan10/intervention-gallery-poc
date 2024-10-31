<template>
  <div class="filter-bar">
    <input v-model="searchTerm" placeholder="Search interventions..." />
    <select v-model="selectedFilter">
      <option value="">All</option>
      <option value="cost">Cost</option>
      <option value="space">Space Requirements</option>
      <option value="environmental">Environmental Focus</option>
    </select>
    <button @click="applyFilter">Apply Filter</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      searchTerm: "",
      selectedFilter: ""
    };
  },
  methods: {
    applyFilter() {
      this.$emit("filter", { searchTerm: this.searchTerm, filter: this.selectedFilter });
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

input, select {
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