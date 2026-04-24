<template>
  <div id="app-container">
    <nav class="navbar">
      <div style="display: flex; align-items: center; gap: 1rem;">
        <img src="/logo.png" alt="Gusteau's Logo" style="height: 60px; width: auto;">
        <h1>Le Gourmet</h1>
      </div>
      <ul class="nav-links">
        <li><router-link to="/" :class="{ active: $route.path === '/' }">Accueil</router-link></li>
        <li><router-link to="/menu" :class="{ active: $route.path === '/menu' }">Menu</router-link></li>
        <li><router-link to="/panier" :class="{ active: $route.path === '/panier' }">Panier ({{ cartCount }})</router-link></li>
        <li><router-link to="/admin" :class="{ active: $route.path === '/admin' }">Admin</router-link></li>
      </ul>
    </nav>

    <router-view v-slot="{ Component }">
      <Transition mode="out-in" name="transition-fade">
        <component :is="Component" />
      </Transition>
    </router-view>

    <transition-group name="toast">
      <div
        v-for="toast in toasts"
        :key="toast.id"
        :class="['toast', toast.type]"
      >
        {{ toast.message }}
      </div>
    </transition-group>
  </div>
</template>

<script>
import { ref, computed, onMounted, provide, watch } from 'vue'

export default {
  name: 'App',
  setup() {
    const dishes = ref([
      { id: 1, name: 'Salade César', description: 'Salade fraîche avec croutons et sauce César', price: 12, isNew: false },
      { id: 2, name: 'Steak Frites', description: 'Steak juteux avec frites maison', price: 25, isNew: true },
      { id: 3, name: 'Soupe à l\'Oignon', description: 'Soupe gratinée gratinée au fromage', price: 10, isNew: false },
      { id: 4, name: 'Moules Marinières', description: 'Moules fraîches cuites à la marinière', price: 18, isNew: true },
      { id: 5, name: 'Poulpe à la Galicienne', description: 'Poulpe tendrement cuit avec huile d\'olive', price: 20, isNew: false },
      { id: 6, name: 'Tarte Tatin', description: 'Délicieuse tarte aux pommes caramélisées', price: 9, isNew: false }
    ])

    const cart = ref([])
    const orders = ref([])
    const toasts = ref([])

    const cartCount = computed(() => {
      return cart.value.reduce((total, item) => total + item.quantity, 0)
    })

    const cartTotal = computed(() => {
      return cart.value.reduce((total, item) => total + (item.price * item.quantity), 0)
    })

    const addToCart = (dish) => {
      const existingItem = cart.value.find(item => item.id === dish.id)
      if (existingItem) {
        existingItem.quantity++
      } else {
        cart.value.push({
          ...dish,
          quantity: 1
        })
      }
      showToast(`${dish.name} ajouté au panier!`, 'success')
    }

    const removeFromCart = (dishId) => {
      cart.value = cart.value.filter(item => item.id !== dishId)
    }

    const updateQuantity = (dishId, quantity) => {
      const item = cart.value.find(item => item.id === dishId)
      if (item) {
        item.quantity = parseInt(quantity)
        if (item.quantity <= 0) {
          removeFromCart(dishId)
        }
      }
    }

    const finalizeOrder = (clientName) => {
      if (cart.value.length === 0) {
        showToast('Le panier est vide!', 'error')
        return false
      }
      
      const order = {
        id: orders.value.length + 1,
        clientName: clientName || 'Client Anonymous',
        items: [...cart.value],
        total: cartTotal.value,
        isReady: false,
        createdAt: new Date().toLocaleString('fr-FR')
      }

      orders.value.push(order)
      cart.value = []
      showToast('Commande finalisée avec succès!', 'success')
      return true
    }

    const markOrderAsReady = (orderId) => {
      const order = orders.value.find(o => o.id === orderId)
      if (order) {
        order.isReady = true
        showToast('Commande marquée comme prête!', 'success')
      }
    }

    const showToast = (message, type = 'success') => {
      const id = Date.now()
      toasts.value.push({ id, message, type })
      setTimeout(() => {
        toasts.value = toasts.value.filter(t => t.id !== id)
      }, 3000)
    }

    provide('dishes', dishes)
    provide('cart', cart)
    provide('orders', orders)
    provide('addToCart', addToCart)
    provide('removeFromCart', removeFromCart)
    provide('updateQuantity', updateQuantity)
    provide('finalizeOrder', finalizeOrder)
    provide('markOrderAsReady', markOrderAsReady)
    provide('cartTotal', cartTotal)

    onMounted(() => {
      console.log('Les moules sont prêtes')
    })

    watch(cart, (newCart) => {
    }, { deep: true })

    return {
      toasts,
      cartCount,
      cartTotal
    }
  }
}
</script>

<style scoped>
#app-container {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}

.toast-group {
  position: fixed;
  bottom: 2rem;
  right: 2rem;
  z-index: 1000;
}

.toast-enter-active,
.toast-leave-active {
  transition: all 0.3s ease;
}

.toast-enter-from {
  transform: translateX(400px);
  opacity: 0;
}

.toast-leave-to {
  transform: translateX(400px);
  opacity: 0;
}
</style>
