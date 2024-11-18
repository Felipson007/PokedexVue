<template>
  <div class="row">
    <div
      class="col-6 col-sm-4 col-md-3 col-lg-2 mb-4"
      v-for="pokemon in pokemons"
      :key="pokemon.id"
    >
      <PokemonCard :pokemon="pokemon" />
    </div>
  </div>
</template>

<script>
import PokemonCard from './PokemonCard.vue';

export default {
  name: 'PokemonGrid',
  components: { PokemonCard },
  props: ['pokemons'],
};
</script>
