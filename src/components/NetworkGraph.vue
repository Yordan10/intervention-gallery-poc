<template>
    <div>
      <div ref="cy" class="cy-container"></div>
      <div v-if="selectedNode" class="info-panel">
        <h3>{{ selectedNode.label }}</h3>
        <p>More details about the intervention could go here.</p>
        <p>Related Interventions:</p>
        <ul>
          <li v-for="related in relatedNodes" :key="related.id">
            {{ related.label }}
          </li>
        </ul>
        <button @click="clearSelection">Close</button>
      </div>
    </div>
  </template>
  
  <script>
  import cytoscape from 'cytoscape';
  
  export default {
    props: {
      nodes: Array,
      links: Array
    },
    data() {
      return {
        cy: null,
        selectedNode: null,
        relatedNodes: []
      };
    },
    mounted() {
      this.initCytoscape();
    },
    methods: {
      initCytoscape() {
        const elements = [
          ...this.nodes.map(node => ({ data: { id: node.id, label: node.name } })),
          ...this.links.map(link => ({
            data: { source: link.source, target: link.target, reltype: link.reltype }
          }))
        ];
  
        this.cy = cytoscape({
          container: this.$refs.cy,
          elements,
          style: [
            {
              selector: 'node',
              style: {
                'label': 'data(label)',
                'background-color': '#2d6187',
                'color': '#fff',
                'text-valign': 'center',
                'text-halign': 'center',
                'width': 20,
                'height': 20,
                'font-size': 12,
                'border-width': 3,
                'border-color': '#5f8aa6',
              }
            },
            {
              selector: 'edge',
              style: {
                'line-color': '#9ecae1',
                'width': 2,
                'target-arrow-color': '#9ecae1',
                'target-arrow-shape': 'triangle'
              }
            }
          ],
          layout: {
            name: 'cose',
            padding: 10
          }
        });
  
        this.cy.on('tap', 'node', (event) => {
          this.selectNode(event.target);
        });
      },
      selectNode(node) {
        // Clear previous highlights
        this.cy.nodes().removeClass('dim');
        this.cy.edges().removeClass('highlight');
  
        // Highlight selected node and related nodes
        this.selectedNode = node.data();
        const relatedEdges = node.connectedEdges();
        relatedEdges.addClass('highlight');
        
        // Dim all nodes, then highlight the selected node and its neighbors
        this.cy.nodes().addClass('dim');
        node.removeClass('dim');
        node.neighborhood().removeClass('dim');
  
        // Gather related nodes information for display
        this.relatedNodes = node.neighborhood('node').map(n => n.data());
      },
      clearSelection() {
        this.selectedNode = null;
        this.relatedNodes = [];
        this.cy.nodes().removeClass('dim');
        this.cy.edges().removeClass('highlight');
      }
    },
    beforeUnmount() {
      if (this.cy) this.cy.destroy();
    }
  };
  </script>
  
  <style scoped>
  .cy-container {
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
  
  .dim {
    opacity: 0.3;
  }
  
  .highlight {
    line-color: #f39c12 !important;
    width: 4px;
  }
  </style>
  