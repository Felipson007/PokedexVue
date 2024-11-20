<template>
  <nav class="navbar">
    <div class="container">
      <a class="navbar-brand" href="#">
        <img src="/src/assets/logo.svg" alt="Pokedex Logo" class="logo" />
      </a>
      <button class="navbar-toggler" @click="toggleMenu">
        ☰
      </button>
      <div class="navbar-links" :class="{ 'is-open': isMenuOpen }">
        <a class="nav-link" href="#">Home</a>
        <a class="nav-link active" href="#">Pokédex</a>
      </div>
    </div>
  </nav>
</template>

<script>
export default {
  data() {
    return {
      isMenuOpen: false,
    };
  },
  methods: {
    toggleMenu() {
      this.isMenuOpen = !this.isMenuOpen;
    },
  },
};
</script>

<style scoped>
.navbar-toggler {
  display: none;
  background: none;
  border: none;
  font-size: 1.5rem;
  cursor: pointer;
}
.navbar {
    display: flex;
    align-items: center;
    position: absolute;
    justify-content: space-between;
    padding: 10px 20px;
    top: 0;
    width: 100%;
    left: 0;
    background-color: #ffffff;
    border-bottom: 2px solid #e2e2e2;
}


.navbar-links {
  display: flex;
  gap: 15px;
}

.navbar-links.is-open {
  display: block;
}

@media (max-width: 768px) {
  .navbar-toggler {
    display: block;
  }
  .navbar-links {
    display: none;
    flex-direction: column;
    gap: 10px;
  }
}
</style>
