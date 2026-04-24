<template>
  <div class="home">
    <div class="container">
      <h2>Bienvenue au Le Gourmet</h2>
      <p>Découvrez notre sélection de plats délicieux préparés avec passion</p>
      <p style="font-size: 1rem; margin-bottom: 3rem; color: #777;">
        Restaurant gastronomique proposant une cuisine raffinée avec des ingrédients frais et de qualité.
      </p>
      <router-link to="/menu">
        <button class="btn">👉 Accéder au Menu</button>
      </router-link>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HomeView'
}
</script>

<style scoped>
.home {
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: center;
  background: linear-gradient(135deg, #f5e6d3 0%, #e8d4b8 100%);
}

.container {
  text-align: center;
}

a {
  text-decoration: none;
}
</style>
