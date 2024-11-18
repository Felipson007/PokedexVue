<template>
  <nav class="d-flex justify-content-center mt-4">
    <ul class="pagination">
      <!-- Botão Anterior -->
      <li class="page-item" :class="{ disabled: currentPage === 1 }">
        <button 
          class="page-link" 
          :aria-disabled="currentPage === 1" 
          aria-label="Página anterior" 
          @click="$emit('changePage', currentPage - 1)"
        >
          Anterior
        </button>
      </li>

      <!-- Botões de páginas -->
      <li 
        class="page-item" 
        v-for="page in totalPages" 
        :key="page" 
        :class="{ active: currentPage === page }"
      >
        <button class="page-link" @click="$emit('changePage', page)">
          {{ page }}
        </button>
      </li>

      <!-- Botão Próximo -->
      <li class="page-item" :class="{ disabled: currentPage === totalPages }">
        <button 
          class="page-link" 
          :aria-disabled="currentPage === totalPages" 
          aria-label="Próxima página" 
          @click="$emit('changePage', currentPage + 1)"
        >
          Próximo
        </button>
      </li>
    </ul>
  </nav>
</template>

<script>
export default {
  name: 'Pagination',
  props: ['currentPage', 'totalPages'],
};
</script>

<style scoped>
/* Estilo geral da paginação */
.pagination {
  display: flex;
  list-style: none;
  padding: 0;
  margin: 0;
}

.page-item {
  margin: 0 5px;
}

.page-item .page-link {
  border: none;
  background: none;
  font-size: 16px;
  font-weight: 400;
  color: #2C2C2C;
  cursor: pointer;
}

.page-item.disabled .page-link {
  color: #6c757d; /* Cor cinza claro para botões desabilitados */
  cursor: not-allowed;
}

.page-item.active .page-link {
  background-color: #2C2C2C;
  color: #ffffff;
  border-radius: 50%;
  width: 36px;
  height: 36px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.page-link {
  padding: 6px 12px;
  text-decoration: none;
}

ul.pagination {
  --bs-pagination-border-color: unset; 
  --bs-pagination-border-radius: unset;
}
</style>
