<template>
    <button
      v-show="visible"
      class="btn btn-primary scroll-to-top"
      @click="scrollToTop"
    >
      ↑
    </button>
  </template>
  
  <script>
  export default {
    name: "ScrollToTopButton",
    data() {
      return {
        visible: false, // Define se o botão está visível ou não
      };
    },
    methods: {
      scrollToTop() {
        window.scrollTo({ top: 0, behavior: "smooth" }); // Volta ao topo suavemente
      },
      handleScroll() {
        this.visible = window.scrollY > 200; // Exibe o botão apenas quando o scroll for maior que 200px
      },
    },
    mounted() {
      window.addEventListener("scroll", this.handleScroll); // Adiciona evento de scroll ao montar o componente
    },
    beforeUnmount() {
      window.removeEventListener("scroll", this.handleScroll); // Remove o evento ao desmontar o componente
    },
  };
  </script>
  
  <style scoped>
  .scroll-to-top {
    position: fixed;
    bottom: 20px;
    right: 20px;
    z-index: 1000;
    background-color: #6c757d; /* Cor do botão (Bootstrap Primary) */
    color: white;
    border: none;
    border-radius: 50%;
    width: 50px;
    height: 50px;
    display: flex;
    align-items: center;
    justify-content: center;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    cursor: pointer;
    font-size: 18px;
  }
  
  .scroll-to-top:hover {
    background-color: #0056b3; /* Cor mais escura no hover */
  }
  </style>
  