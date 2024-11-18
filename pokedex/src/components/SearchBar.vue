<template>
  <div class="search-container">
    <input
      type="text"
      class="search-input"
      placeholder="Faça uma busca pelo nome do Pokémon"
      v-model="query"
      @input="emitSearch"
    />
    <i class="fa-solid fa-magnifying-glass search-icon"></i>
  </div>
</template>

<script>
export default {
  name: 'SearchBar',
  data() {
    return {
      query: '',
    };
  },
  methods: {
    emitSearch() {
      this.$emit('onSearch', this.query);
    },
  },
};
</script>

<style scoped>
.search-container {
  position: relative;
  display: flex;
  align-items: center;
  background-color: #f6f6fc;
  border-radius: 24px;
  margin: 2.5rem;
  padding: 0 16px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.search-input {
  flex: 1;
  border: none;
  outline: none;
  background-color: transparent;
  font-size: 16px;
  color: #6c757d;
  padding: 12px 16px;
}

.search-input::placeholder {
  color: #aaa;
}

.search-icon {
  color: #6c757d;
  cursor: pointer;
  margin-left: -8px;
}

.search-container:focus-within {
  box-shadow: 0 0 0 2px #e2e8f0;
}

</style>
