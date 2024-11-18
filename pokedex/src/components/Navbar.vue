<template>
  <nav class="navbar">
    <div class="container">
      <a class="navbar-brand" href="#">
        <img src="/src/assets/logo.svg" alt="Pokedex Logo" class="logo" />
      </a>
      <div class="navbar-links">
        <a class="nav-link" href="#">Home</a>
        <a class="nav-link active" href="#">Pokédex</a>
      </div>
    </div>
  </nav>
</template>

<script>
export default {
  name: 'Navbar',
};
</script>

<style scoped>
.navbar[data-v-4d808a77] {
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

.logo {
  height: 40px;
  max-width: 100px;
}


.navbar-links {
  display: flex;
  gap: 15px; 
}

.nav-link {
  font-size: 16px;
  color: #6c757d; /* Cinza escuro */
  text-decoration: none;
  font-weight: 500;
}

.nav-link:hover {
  color: #2c2c2c; /* Preto ao passar o mouse */
}

.nav-link.active {
  font-weight: 700; /* Negrito para o link ativo */
  color: #2c2c2c; /* Preto para o ativo */
}
</style>
