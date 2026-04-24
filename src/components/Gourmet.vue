<template>
  <div class="menu-item">
    <div class="menu-item-header">
      <slot name="victuaille" id="victuaille"></slot>
      <h3>{{ dish.name }}</h3>
      <div class="price">{{ dish.price }}€</div>
    </div>
    <div class="menu-item-body">
      <p>{{ dish.description }}</p>
    </div>
    <div class="menu-item-footer">
      <button class="btn" @click="emitFestoyons">
        🛒 Ajouter au panier
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Gourmet',
  props: {
    dish: {
      type: Object,
      required: true
    }
  },
  emits: ['festoyons'],
  setup(props, { emit }) {
    const emitFestoyons = () => {
      emit('festoyons', props.dish)
    }
    return {
      emitFestoyons
    }
  }
}
</script>

<style scoped>
.menu-item {
  background: white;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 4px 8px rgba(0,0,0,0.1);
  transition: transform 0.3s;
  display: flex;
  flex-direction: column;
  height: 100%;
}

.menu-item:hover {
  transform: translateY(-5px);
  box-shadow: 0 6px 12px rgba(0,0,0,0.15);
}

.menu-item-header {
  background-color: #f0e68c;
  padding: 1rem;
  position: relative;
}

.menu-item-header h3 {
  margin: 0 0 0.5rem 0;
  color: #8B4513;
}

.menu-item-header .price {
  font-size: 1.5rem;
  font-weight: bold;
  color: #8B4513;
}

.badge {
  position: absolute;
  top: 10px;
  right: 10px;
  background-color: #ff6b6b;
  color: white;
  padding: 0.25rem 0.75rem;
  border-radius: 20px;
  font-size: 0.75rem;
  font-weight: bold;
}

.menu-item-body {
  padding: 1rem;
  text-align: left;
  flex: 1;
}

.menu-item-body p {
  color: #666;
  margin: 0;
  line-height: 1.5;
}

.menu-item-footer {
  padding: 1rem;
  text-align: center;
  border-top: 1px solid #eee;
}

.btn {
  width: 100%;
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
</style>
