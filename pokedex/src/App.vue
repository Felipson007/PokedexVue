<template>
    <Navbar />
    <div class="container my-5 page">
      <SearchBar @onSearch="searchPokemon" />
      <div v-if="isSearching" class="text-center my-4">Buscando Pokémon...</div>
      <div v-else-if="filteredPokemons.length === 0">
        <div class="text-center my-4 text-danger">
          Nenhum Pokémon encontrado.
        </div>
      </div>
      <div v-else>
        <PokemonGrid :pokemons="filteredPokemons" />
        <Pagination
          :currentPage="currentPage"
          :totalPages="totalPages"
          @changePage="changePage"
        />
      </div>
    </div>
  </template>
  
  <script>
  import Navbar from './components/Navbar.vue';
  import SearchBar from './components/SearchBar.vue';
  import PokemonGrid from './components/PokemonGrid.vue';
  import Pagination from './components/Pagination.vue';
  
  export default {
    components: { Navbar, SearchBar, PokemonGrid, Pagination },
    data() {
      return {
        pokemons: [],
        filteredPokemons: [],
        searchQuery: '',
        isSearching: false,
        currentPage: 1,
        totalPages: 5,
      };
    },
    methods: {
      async fetchPokemons() {
        const offset = (this.currentPage - 1) * 20;
        const response = await fetch(
          `https://pokeapi.co/api/v2/pokemon?limit=20&offset=${offset}`
        );
        const data = await response.json();
  
        this.pokemons = await Promise.all(
          data.results.map(async (pokemon, index) => {
            const pokemonDetails = await fetch(pokemon.url).then((res) =>
              res.json()
            );
            const types = pokemonDetails.types.map(
              (typeInfo) => typeInfo.type.name
            );
  
            return {
              id: offset + index + 1,
              name: pokemon.name,
              image: `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${offset + index + 1}.png`,
              types,
            };
          })
        );
  
        this.filteredPokemons = [...this.pokemons];
      },
  
      searchPokemon(query) {
        this.searchQuery = query.toLowerCase();
  
        if (!this.searchQuery) {
          this.filteredPokemons = [...this.pokemons];
          return;
        }
  
        this.filteredPokemons = this.pokemons.filter((pokemon) =>
          pokemon.name.toLowerCase().includes(this.searchQuery)
        );
      },
  
      changePage(page) {
        this.currentPage = page;
        this.fetchPokemons();
      },
    },
    mounted() {
      this.fetchPokemons();
    },
  };
  </script>
<style>
.page{
    margin-top: 8rem !important;
}
</style>