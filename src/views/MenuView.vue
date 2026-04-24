<template>
  <div class="container">
    <h2>📋 Notre Menu</h2>
    <div class="menu-grid">
      <Gourmet
        v-for="dish in dishes"
        :key="dish.id"
        :dish="dish"
        @festoyons="handleAddToCart"
      >
        <template #victuaille v-if="dish.isNew">
          <span class="badge">Nouveau</span>
        </template>
      </Gourmet>
    </div>
  </div>
</template>

<script>
import { inject } from 'vue'
import Gourmet from '../components/Gourmet.vue'

export default {
  name: 'MenuView',
  components: {
    Gourmet
  },
  setup() {
    const dishes = inject('dishes')
    const addToCart = inject('addToCart')

    const handleAddToCart = (dish) => {
      addToCart(dish)
    }

    return {
      dishes,
      handleAddToCart
    }
  }
}
</script>

<style scoped>
.container {
  min-height: 100vh;
  padding-bottom: 2rem;
}

h2 {
  color: #8B4513;
  margin-bottom: 2rem;
  text-align: center;
}
</style>
