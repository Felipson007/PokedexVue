<template>
  <Navbar />
  <div class="container my-5 page">
    <SearchBar @onSearch="searchPokemon" />
    <FilterBar @onFilter="filterByType" />
    <div v-if="isSearching" class="text-center my-4">Buscando Pokémon...</div>
    <div v-else-if="filteredPokemons.length === 0">
      <div class="text-center my-4 text-danger">Nenhum Pokémon encontrado.</div>
    </div>
    <div v-else>
      <PokemonGrid :pokemons="paginatedPokemons" />
      <Pagination :currentPage="currentPage" :totalPages="totalPages" @changePage="changePage" />
    </div>
  </div>
  <ScrollToTopButton />
</template>

<script>
import Navbar from './components/Navbar.vue';
import SearchBar from './components/SearchBar.vue';
import PokemonGrid from './components/PokemonGrid.vue';
import Pagination from './components/Pagination.vue';
import FilterBar from './components/FilterBar.vue';
import ScrollToTopButton from "./components/ScrollToTopButton.vue";

export default {
  components: { Navbar, SearchBar, PokemonGrid, Pagination, FilterBar, ScrollToTopButton },
  data() {
    return {
      pokemons: [],
      filteredPokemons: [],
      searchQuery: '',
      selectedType: '',
      isSearching: false,
      currentPage: 1,
      totalPages: 5,
      pageSize: 30,
      showScrollTopButton: false,
    };
  },
  computed: {
    paginatedPokemons() {
      const start = (this.currentPage - 1) * this.pageSize;
      const end = start + this.pageSize;
      return this.filteredPokemons.slice(start, end);
    },
  },
  methods: {
    async fetchPokemons() {
      this.isSearching = true;
      try {
        const response = await fetch(
          `https://pokeapi.co/api/v2/pokemon?limit=150`
        );
        const data = await response.json();

        this.pokemons = await Promise.all(
          data.results.map(async (pokemon) => {
            const pokemonDetails = await fetch(pokemon.url).then((res) =>
              res.json()
            );
            const types = pokemonDetails.types.map(
              (typeInfo) => typeInfo.type.name
            );
            const image =
              pokemonDetails.sprites.front_default ||
              'https://via.placeholder.com/150?text=No+Image';

            return {
              id: pokemonDetails.id,
              name: pokemonDetails.name,
              image,
              types,
            };
          })
        );

        this.filteredPokemons = [...this.pokemons];
        this.totalPages = Math.ceil(this.filteredPokemons.length / this.pageSize);
      } catch (error) {
        console.error('Erro ao buscar Pokémon:', error);
      } finally {
        this.isSearching = false;
      }
    },

    filterByType(type) {
      this.selectedType = type;
      this.currentPage = 1;

      if (!type) {
        this.filteredPokemons = [...this.pokemons];
      } else {
        this.filteredPokemons = this.pokemons.filter((pokemon) =>
          pokemon.types.includes(type)
        );
      }

      if (this.searchQuery) {
        this.applySearchToFiltered();
      }

      this.totalPages = Math.ceil(this.filteredPokemons.length / this.pageSize);
    },

    searchPokemon(query) {
      this.searchQuery = query.toLowerCase();
      this.currentPage = 1;

      this.applySearchToFiltered();
    },

    applySearchToFiltered() {
      if (!this.searchQuery) {
        this.filteredPokemons =
          this.selectedType && this.selectedType !== ''
            ? this.pokemons.filter((pokemon) =>
              pokemon.types.includes(this.selectedType)
            )
            : [...this.pokemons];
      } else {
        this.filteredPokemons = this.filteredPokemons.filter((pokemon) =>
          pokemon.name.toLowerCase().includes(this.searchQuery)
        );
      }

      this.totalPages = Math.ceil(this.filteredPokemons.length / this.pageSize);
    },

    changePage(page) {
      this.currentPage = page;
    },

  },
  mounted() {
    this.fetchPokemons();
  },
};
</script>

<style>
.page {
  margin-top: 8rem !important;
  padding: 0 1rem;
}

body {
  overflow-y: scroll;
  scroll-behavior: smooth;
}

body::-webkit-scrollbar {
  width: 12px;
}

body::-webkit-scrollbar-thumb {
  background-color: #6c757d;
  border-radius: 10px;
}

body::-webkit-scrollbar-thumb:hover {
  background-color: #495057;
}

body::-webkit-scrollbar-track {
  background-color: #f8f9fa;
}

@media (max-width: 768px) {
  .scroll-to-top {
    font-size: 12px;
    padding: 8px 12px;
  }
}
</style>
