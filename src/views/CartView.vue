<template>
  <div class="container">
    <h2>🛒 Mon Panier</h2>

    <div v-if="cart.length === 0" class="empty-state">
      <p>Votre panier est vide</p>
      <router-link to="/menu">
        <button class="btn">Retour au menu</button>
      </router-link>
    </div>

    <div v-else>
      <table class="cart-table">
        <thead>
          <tr>
            <th>Plat</th>
            <th>Prix</th>
            <th>Quantité</th>
            <th>Sous-total</th>
            <th>Action</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="item in cart" :key="item.id">
            <td>{{ item.name }}</td>
            <td>{{ item.price }}€</td>
            <td>
              <div class="quantity-control">
                <button @click="updateQuantity(item.id, item.quantity - 1)">-</button>
                <input type="number" v-model.number="item.quantity" @change="updateQuantity(item.id, item.quantity)" />
                <button @click="updateQuantity(item.id, item.quantity + 1)">+</button>
              </div>
            </td>
            <td>{{ (item.price * item.quantity).toFixed(2) }}€</td>
            <td>
              <button class="btn btn-danger" @click="removeFromCart(item.id)">Supprimer</button>
            </td>
          </tr>
        </tbody>
      </table>

      <div class="cart-summary">
        <div style="margin-bottom: 1rem;">
          <span>Sous-total: </span>
          <strong>{{ cartTotal.toFixed(2) }}€</strong>
        </div>
      </div>

      <div style="margin-top: 2rem; text-align: right;">
        <div class="input-group" style="display: inline-block; text-align: left; max-width: 300px;">
          <label for="client-name">Nom du client :</label>
          <input v-model="clientName" id="client-name" type="text" placeholder="Entrez votre nom" />
        </div>
        <div style="margin-top: 1rem;">
          <button class="btn btn-success" @click="handleFinalizeOrder" style="margin-right: 1rem;">
            ✅ Finaliser la commande
          </button>
          <router-link to="/menu">
            <button class="btn">Continuer mes achats</button>
          </router-link>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { inject, ref, computed } from 'vue'
import { useRouter } from 'vue-router'

export default {
  name: 'CartView',
  setup() {
    const router = useRouter()
    const cart = inject('cart')
    const cartTotal = inject('cartTotal')
    const removeFromCart = inject('removeFromCart')
    const updateQuantity = inject('updateQuantity')
    const finalizeOrder = inject('finalizeOrder')

    const clientName = ref('')

    const handleFinalizeOrder = () => {
      if (!clientName.value.trim()) {
        alert('Veuillez entrer votre nom')
        return
      }
      finalizeOrder(clientName.value)
      clientName.value = ''
      router.push('/')
    }

    return {
      cart,
      cartTotal,
      removeFromCart,
      updateQuantity,
      handleFinalizeOrder,
      clientName
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
}

.empty-state {
  text-align: center;
  padding: 3rem 2rem;
  color: #999;
}

.empty-state p {
  font-size: 1.1rem;
  margin-bottom: 2rem;
}

.cart-table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 2rem;
  background: white;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 2px 8px rgba(0,0,0,0.1);
}

.cart-table th,
.cart-table td {
  padding: 1rem;
  text-align: left;
  border-bottom: 1px solid #ddd;
}

.cart-table th {
  background-color: #8B4513;
  color: white;
  font-weight: bold;
}

.cart-table tr:hover {
  background-color: #f9f9f9;
}

.quantity-control {
  display: flex;
  gap: 0.5rem;
  align-items: center;
}

.quantity-control button {
  width: 30px;
  height: 30px;
  padding: 0;
  font-size: 1rem;
  background-color: #8B4513;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.quantity-control button:hover {
  background-color: #A0522D;
}

.quantity-control input {
  width: 50px;
  padding: 0.25rem;
  text-align: center;
  border: 1px solid #ddd;
  border-radius: 4px;
}

.cart-summary {
  text-align: right;
  margin-top: 2rem;
  font-size: 1.2rem;
  padding: 1.5rem;
  background: white;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0,0,0,0.1);
}

.cart-summary strong {
  color: #8B4513;
  font-size: 1.5rem;
}

.btn {
  background-color: #8B4513;
  color: white;
  border: none;
  padding: 0.75rem 1.5rem;
  border-radius: 4px;
  cursor: pointer;
  font-size: 1rem;
  transition: background-color 0.3s;
}

.btn:hover {
  background-color: #A0522D;
}

.btn-danger {
  background-color: #dc3545;
}

.btn-danger:hover {
  background-color: #c82333;
}

.btn-success {
  background-color: #28a745;
  padding: 0.75rem 2rem;
}

.btn-success:hover {
  background-color: #218838;
}

.input-group {
  margin: 1rem 0;
}

.input-group label {
  display: block;
  margin-bottom: 0.5rem;
  font-weight: bold;
  color: #8B4513;
}

.input-group input {
  width: 100%;
  padding: 0.75rem;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 1rem;
}

.input-group input:focus {
  outline: none;
  border-color: #8B4513;
  box-shadow: 0 0 0 3px rgba(139, 69, 19, 0.1);
}
</style>
