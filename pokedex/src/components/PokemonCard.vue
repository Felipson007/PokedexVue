<template>
  <div class="card shadow-sm border-0 p-3 text-center">
    <p class="text-muted mb-0 id">#{{ pokemon.id }}</p>
    <img
      :src="pokemon.image"
      class="card-img-top mx-auto"
      :alt="pokemon.name"
      style="width: 100px;"
    />
    <div class="card-body p-2">
      <div class="mb-2">
        <span
          v-for="type in pokemon.types"
          :key="type"
          class="badge me-1"
          :style="{ backgroundColor: getTypeColor(type), color: '#fff' }"
        >
          {{ type }}
        </span>
      </div>
      <h6 class="fw-bold mb-0">{{ pokemon.name }}</h6>
    </div>
  </div>
</template>

<script>
export default {
  name: 'PokemonCard',
  props: ['pokemon'],
  methods: {
    getTypeColor(type) {
      const typeColors = {
        grass: '#78C850',
        fire: '#F08030',
        water: '#6890F0',
        bug: '#A8B820',
        normal: '#A8A878',
        poison: '#A040A0',
        electric: '#F8D030',
        ground: '#E0C068',
        fairy: '#EE99AC',
        fighting: '#C03028',
        psychic: '#F85888',
        rock: '#B8A038',
        ghost: '#705898',
        ice: '#98D8D8',
        dragon: '#7038F8',
        dark: '#705848',
        steel: '#B8B8D0',
        flying: '#A890F0',
      };

      return typeColors[type.toLowerCase()] || '#A8A8A8'; 
    },
  },
};
</script>

<style scoped>
.card {
  border-radius: 10px;
  background: #F0F3FF;
}
.badge {
  font-size: 12px;
  padding: 5px 10px;
  border-radius: 12px;
}
.id[data-v-63ebbf9b] {
    display: flex;
    justify-content: flex-end;
   
}
</style>
