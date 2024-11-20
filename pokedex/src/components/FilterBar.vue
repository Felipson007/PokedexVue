<template>
  <div class="filter-bar">
    <label for="type">Filtrar por Tipo:</label>
    <div class="dropdown">
      <button
        class="btn btn-secondary dropdown-toggle"
        type="button"
        @click="toggleDropdown"
      >
        {{ selectedTypeLabel }}
      </button>
      <ul
        class="dropdown-menu"
        :class="{ show: dropdownOpen }"
        style="max-height: 200px; overflow-y: auto;"
      >
        <li>
          <a
            class="dropdown-item"
            href="#"
            :class="{ active: selectedType === '' }"
            @click="selectType('')"
          >
            Todos
          </a>
        </li>
        <li v-for="type in types" :key="type.name">
          <a
            class="dropdown-item"
            href="#"
            :class="{ active: selectedType === type.name }"
            @click="selectType(type.name)"
          >
            {{ type.name }}
          </a>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      types: [],
      selectedType: '',
      dropdownOpen: false,
    };
  },
  computed: {
    selectedTypeLabel() {
      return this.selectedType || 'Todos';
    },
  },
  methods: {
    toggleDropdown() {
      this.dropdownOpen = !this.dropdownOpen;
    },
    selectType(type) {
      this.selectedType = type;
      this.$emit('onFilter', type);
      this.dropdownOpen = false; // Fecha o dropdown ao selecionar
    },
    async fetchTypes() {
      try {
        const response = await fetch('https://pokeapi.co/api/v2/type');
        const data = await response.json();
        this.types = data.results;
      } catch (error) {
        console.error('Erro ao carregar tipos:', error);
      }
    },
  },
  mounted() {
    this.fetchTypes();
  },
};
</script>

<style scoped>
/* Estilo do menu do dropdown */
.dropdown-menu {
  max-height: 200px; /* Limita a altura */
  overflow-y: auto; /* Adiciona scroll vertical */
}

/* Personalizar a barra de rolagem */
.dropdown-menu::-webkit-scrollbar {
  width: 8px; /* Largura da barra */
}

.dropdown-menu::-webkit-scrollbar-thumb {
  background-color: #6c757d; /* Cor da barra */
  border-radius: 10px; /* Bordas arredondadas */
}

.dropdown-menu::-webkit-scrollbar-thumb:hover {
  background-color: #495057; /* Cor ao passar o mouse */
}

.dropdown-menu::-webkit-scrollbar-track {
  background-color: #f8f9fa; /* Fundo do track */
}
.filter-bar {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin: 1.5em 0;
}

.dropdown-menu {
  max-height: 200px;
  overflow-y: auto;
}

.dropdown-toggle {
  min-width: 200px;
  text-align: center;
}

@media (max-width: 768px) {
  .filter-bar {
    width: 100%;
  }

  .dropdown-toggle {
    width: 100%;
  }
}
</style>
