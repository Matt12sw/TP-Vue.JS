<template>
  <div class="container">
    <h2>Gestion des Commandes (Admin)</h2>

    <div v-if="orders.length === 0" class="empty-state">
      <p>Aucune commande en attente</p>
    </div>

    <ul v-else class="admin-list">
      <li v-for="order in orders" :key="order.id" :class="['admin-item', { ready: order.isReady }]">
        <div style="display: flex; justify-content: space-between; align-items: start;">
          <div style="flex: 1;">
            <h4>Commande #{{ order.id }}</h4>
            <span :class="['status', order.isReady ? 'ready' : 'pending']">
              {{ order.isReady ? '✅ Prête' : '⏳ En cours' }}
            </span>
            <div style="margin-top: 0.5rem; font-size: 0.9rem; color: #666;">
              <p><strong>Client:</strong> {{ order.clientName }}</p>
              <p><strong>Créée:</strong> {{ order.createdAt }}</p>
            </div>
            <div style="margin-top: 1rem;">
              <p style="margin-bottom: 0.5rem; font-weight: bold;">Plats commandés:</p>
              <ul>
                <li v-for="item in order.items" :key="item.id">
                  {{ item.name }} (x{{ item.quantity }}) - {{ (item.price * item.quantity).toFixed(2) }}€
                </li>
              </ul>
            </div>
            <div style="margin-top: 1rem; padding-top: 1rem; border-top: 1px solid #ddd;">
              <strong>Total: {{ order.total.toFixed(2) }}€</strong>
            </div>
          </div>
          <button
            v-if="!order.isReady"
            class="btn btn-success"
            @click="markAsReady(order.id)"
            style="margin-left: 1rem; white-space: nowrap;"
          >
            Marquer prête
          </button>
          <span v-else style="margin-left: 1rem; color: #28a745; font-weight: bold;">
            ✓ Prête
          </span>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
import { inject } from 'vue'

export default {
  name: 'AdminView',
  setup() {
    const orders = inject('orders')
    const markOrderAsReady = inject('markOrderAsReady')

    const markAsReady = (orderId) => {
      markOrderAsReady(orderId)
    }

    return {
      orders,
      markAsReady
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
}

.admin-list {
  list-style: none;
  margin: 0;
  padding: 0;
}

.admin-item {
  background: white;
  border-left: 4px solid #8B4513;
  padding: 1.5rem;
  margin-bottom: 1rem;
  border-radius: 4px;
  box-shadow: 0 2px 8px rgba(0,0,0,0.1);
  transition: transform 0.3s, box-shadow 0.3s;
}

.admin-item:hover {
  transform: translateX(5px);
  box-shadow: 0 4px 12px rgba(0,0,0,0.15);
}

.admin-item.ready {
  border-left-color: #28a745;
  background-color: #f0fdf4;
}

.admin-item h4 {
  margin: 0 0 0.5rem 0;
  color: #8B4513;
}

.status {
  display: inline-block;
  padding: 0.25rem 0.75rem;
  border-radius: 20px;
  font-size: 0.85rem;
  font-weight: bold;
}

.status.pending {
  background-color: #fff3cd;
  color: #856404;
}

.status.ready {
  background-color: #d4edda;
  color: #155724;
}

.admin-item ul {
  list-style: none;
  margin: 0;
  padding-left: 0;
}

.admin-item li {
  padding: 0.5rem 0;
  border-bottom: 1px solid #eee;
  color: #666;
}

.admin-item li:last-child {
  border-bottom: none;
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

.btn-success {
  background-color: #28a745;
}

.btn-success:hover {
  background-color: #218838;
}
</style>
