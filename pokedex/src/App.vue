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
      <Pagination :currentPage="currentPage" :totalPages="totalPages" @changePage="changePage" />
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

          // Tipos e imagem diretamente dos dados da API
          const types = pokemonDetails.types.map((typeInfo) => typeInfo.type.name);
          const image = pokemonDetails.sprites.front_default || 'https://via.placeholder.com/150?text=No+Image';

          return {
            id: pokemonDetails.id,
            name: pokemonDetails.name,
            image, // Usa a URL da imagem ou fallback
            types,
          };
        })
      );

      this.filteredPokemons = [...this.pokemons]; // Inicializa a lista filtrada
    },

    async searchPokemon(query) {
      this.searchQuery = query.toLowerCase();

      if (!this.searchQuery) {
        // Se a busca estiver vazia, exibe os Pokémon da página atual
        this.filteredPokemons = [...this.pokemons];
        return;
      }

      this.isSearching = true;

      try {
        // Busca todos os Pokémon para realizar a pesquisa
        let nextUrl = 'https://pokeapi.co/api/v2/pokemon?limit=100';
        const allPokemons = [];

        while (nextUrl) {
          const response = await fetch(nextUrl);
          const data = await response.json();
          allPokemons.push(...data.results);
          nextUrl = data.next;
        }

        const matchedPokemons = allPokemons.filter((pokemon) =>
          pokemon.name.toLowerCase().includes(this.searchQuery)
        );

        // Busca detalhes dos Pokémon encontrados
        this.filteredPokemons = await Promise.all(
          matchedPokemons.map(async (pokemon) => {
            const pokemonDetails = await fetch(pokemon.url).then((res) =>
              res.json()
            );
            const types = pokemonDetails.types.map(
              (typeInfo) => typeInfo.type.name
            );

            return {
              id: pokemonDetails.id,
              name: pokemonDetails.name,
              image: `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${pokemonDetails.id}.png`,
              types,
            };
          })
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
      this.fetchPokemons(); // Recarrega os Pokémon da página atual
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
}
</style>