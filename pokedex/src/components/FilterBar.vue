<template>
  <div class="filter-bar dropdown" ref="dropdownContainer">
    <label class="labelText" for="type">Filtrar por Tipo de Pokémon:</label>
    <button
      class="btn btn-secondary dropdown-toggle"
      type="button"
      id="dropdownMenuButton"
      @click="toggleDropdown"
    >
      {{ selectedTypeLabel }}
    </button>
    <ul
      class="dropdown-menu"
      :class="{ show: dropdownOpen }"
      aria-labelledby="dropdownMenuButton"
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
</template>

<script>
export default {
  data() {
    return {
      types: [], // Lista de tipos
      selectedType: '', // Tipo selecionado
      dropdownOpen: false, // Controle do estado de abertura do dropdown
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
    handleClickOutside(event) {
      if (this.$refs.dropdownContainer && !this.$refs.dropdownContainer.contains(event.target)) {
        this.dropdownOpen = false; // Fecha o dropdown se o clique for fora
      }
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
    document.addEventListener('click', this.handleClickOutside); // Adiciona o evento de clique no documento
  },
  beforeUnmount() {
    document.removeEventListener('click', this.handleClickOutside); // Remove o evento ao desmontar o componente
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
  margin: 1.5em 0;
}

/* Estilo do menu do dropdown */
.dropdown-menu {
  max-height: 200px; /* Limita a altura */
  overflow-y: auto; /* Adiciona scroll vertical */
  width: 100%; /* Garante que o dropdown ocupe toda a largura do botão */
  left: 0; /* Alinha o dropdown ao início do botão */
  top: 100%; /* Coloca o dropdown exatamente abaixo do botão */
  border-radius: 0 0 8px 8px; /* Adiciona bordas arredondadas na parte inferior */
  border: 1px solid #ddd; /* Bordas consistentes */
}

/* Botão do dropdown */
.dropdown-toggle {
  min-width: 200px; /* Define um tamanho mínimo para o botão */
  width: 100%; /* Faz com que o botão ocupe toda a largura disponível */
  text-align: center;
}

.labelText{
  text-align: center;
  margin-bottom: 5px;
}

/* Para dispositivos móveis */
@media (max-width: 768px) {
  .filter-bar {
    width: 100%;
  }

  .dropdown-toggle {
    width: 100%; /* O botão ocupa toda a largura */
  }

  .dropdown-menu {
    width: 100%; /* O dropdown ocupa a mesma largura do botão */
  }
}
</style>
