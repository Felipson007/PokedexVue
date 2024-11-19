<template>
    <div class="filter-bar">
      <label for="type">Filtrar por Tipo:</label>
      <select id="type" v-model="selectedType" @change="emitFilter">
        <option value="">Todos</option>
        <option v-for="type in types" :key="type.name" :value="type.name">
          {{ type.name }}
        </option>
      </select>
    </div>
  </template>
  
  <script>
  export default {
    name: 'FilterBar',
    data() {
      return {
        types: [], // Tipos de Pokémon carregados da API
        selectedType: '', // Tipo selecionado no filtro
      };
    },
    methods: {
      emitFilter() {
        this.$emit('onFilter', this.selectedType);
      },
      async fetchTypes() {
        try {
          const response = await fetch('https://pokeapi.co/api/v2/type');
          const data = await response.json();
          this.types = data.results; // Armazena os tipos carregados
        } catch (error) {
          console.error('Erro ao carregar tipos:', error);
        }
      },
    },
    mounted() {
      this.fetchTypes(); // Carrega os tipos ao montar o componente
    },
  };
  </script>
  
  <style scoped>
  .filter-bar {
    margin-left: 16px;
    display: flex;
    flex-direction: column;
    align-items: flex-start;
  }
  
  label {
    font-weight: bold;
    margin-bottom: 4px;
  }
  
  select {
    padding: 8px;
    font-size: 14px;
    border-radius: 8px;
    border: 1px solid #ddd;
  }
  </style>
  