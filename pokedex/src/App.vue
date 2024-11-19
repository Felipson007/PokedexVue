<template>
  <Navbar />
  <div class="container my-5 page">
    <div class="search">
      <SearchBar @onSearch="searchPokemon" />
      <FilterBar @onFilter="filterByType" />
    </div>

    <div v-if="isSearching" class="text-center my-4">Buscando Pokémon...</div>
    <div v-else-if="filteredPokemons.length === 0">
      <div class="text-center my-4 text-danger">Nenhum Pokémon encontrado.</div>
    </div>
    <div v-else>
      <PokemonGrid :pokemons="filteredPokemons" />
      <Pagination :currentPage="currentPage" :totalPages="totalPages" @changePage="changePage" />
    </div>
  </div>
</template>

<script>
import Navbar from './components/Navbar.vue';
import SearchBar from './components/SearchBar.vue';
import PokemonGrid from './components/PokemonGrid.vue';
import Pagination from './components/Pagination.vue';
import FilterBar from './components/FilterBar.vue';

export default {
  components: { Navbar, SearchBar, PokemonGrid, Pagination, FilterBar },
  data() {
    return {
      pokemons: [], // Pokémon carregados na página atual
      filteredPokemons: [], // Pokémon filtrados (incluindo por busca e tipo)
      searchQuery: '',
      isSearching: false,
      currentPage: 1,
      totalPages: 5,
      allPokemonsLoaded: false, // Indica se todos os Pokémon foram carregados
    };
  },
  methods: {
    async fetchPokemons() {
      this.isSearching = true;
      const offset = (this.currentPage - 1) * 30; // Limite de 30 por página
      const response = await fetch(
        `https://pokeapi.co/api/v2/pokemon?limit=30&offset=${offset}`
      );
      const data = await response.json();

      this.pokemons = await Promise.all(
        data.results.map(async (pokemon) => {
          const pokemonDetails = await fetch(pokemon.url).then((res) =>
            res.json()
          );

          const types = pokemonDetails.types.map((typeInfo) => typeInfo.type.name);
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
      this.isSearching = false;
    },

    async fetchAllPokemons() {
      // Método para carregar todos os Pokémon (usado na busca ou filtro)
      if (this.allPokemonsLoaded) return;

      this.isSearching = true;
      let nextUrl = 'https://pokeapi.co/api/v2/pokemon?limit=100';
      const allPokemons = [];

      while (nextUrl) {
        const response = await fetch(nextUrl);
        const data = await response.json();
        allPokemons.push(...data.results);
        nextUrl = data.next;
      }

      this.pokemons = await Promise.all(
        allPokemons.map(async (pokemon) => {
          const pokemonDetails = await fetch(pokemon.url).then((res) =>
            res.json()
          );

          const types = pokemonDetails.types.map((typeInfo) => typeInfo.type.name);
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

      this.allPokemonsLoaded = true; // Marca que todos os Pokémon foram carregados
      this.filteredPokemons = [...this.pokemons];
      this.isSearching = false;
    },

    async filterByType(type) {
      if (!type) {
        // Remove o filtro
        this.filteredPokemons = [...this.pokemons];
        return;
      }

      this.isSearching = true;

      try {
        // Carrega todos os Pokémon (se ainda não carregados)
        await this.fetchAllPokemons();

        // Filtra os Pokémon localmente pelo tipo
        this.filteredPokemons = this.pokemons.filter((pokemon) =>
          pokemon.types.includes(type)
        );
      } catch (error) {
        console.error('Erro ao filtrar por tipo:', error);
        this.filteredPokemons = [];
      } finally {
        this.isSearching = false;
      }
    },

    async searchPokemon(query) {
      this.searchQuery = query.toLowerCase();

      if (!this.searchQuery) {
        this.filteredPokemons = [...this.pokemons];
        return;
      }

      this.isSearching = true;

      try {
        await this.fetchAllPokemons(); // Carrega todos os Pokémon para realizar a busca
        this.filteredPokemons = this.pokemons.filter((pokemon) =>
          pokemon.name.toLowerCase().includes(this.searchQuery)
        );
      } catch (error) {
        console.error('Erro ao buscar Pokémon:', error);
        this.filteredPokemons = [];
      } finally {
        this.isSearching = false;
      }
    },

    changePage(page) {
      this.currentPage = page;
      this.fetchPokemons(); // Carrega a página atual
    },
  },

  mounted() {
    this.fetchPokemons(); // Inicializa com os Pokémon da página 1
  },
};
</script>

<style>
.page {
  margin-top: 8rem !important;
  padding: 0 1rem; /* Para mobile */
}
.search{
  display: flex;
  
}
/* Ajustes para mobile */
@media (max-width: 768px) {
  .search-container {
    margin-top: 2rem;
  }
  .filter-bar {
    margin: 1rem 0;
  }
  .pagination {
    justify-content: center;
    flex-wrap: wrap;
  }
}
</style>
