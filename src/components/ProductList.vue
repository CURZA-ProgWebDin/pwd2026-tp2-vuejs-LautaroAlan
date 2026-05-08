<template>
  <div class="list-container">
    <h2>Inventario de Productos</h2>

    <div class="summary-cards">
      <div class="card">
        <h3>Total Productos</h3>
        <p class="number">{{ totalProductsCount }}</p>
      </div>
      <div class="card">
        <h3>Valor Total Inventario</h3>
        <p class="number">${{ totalInventoryValue.toFixed(2) }}</p>
      </div>
    </div>

    <div class="filter-group">
      <label for="category-filter">Filtrar por categoría:</label>
      <select id="category-filter" v-model="selectedCategory">
        <option value="Todos">Mostrar todos</option>
        <option value="Electronica">Electrónica</option>
        <option value="Hardware">Hardware</option>
        <option value="Accesorios">Accesorios</option>
      </select>
    </div>

    <table class="product-table">
      <thead>
        <tr>
          <th>ID</th>
          <th>Nombre</th>
          <th>Categoría</th>
          <th>Precio</th>
          <th>Stock</th>
          <th>Acciones</th>
        </tr>
      </thead>
      <tbody>
        <ProductItem 
          v-for="product in filteredProducts" 
          :key="product.id" 
          :product="product"
          @delete-product="$emit('delete-product', $event)"
        />

        <tr v-if="filteredProducts.length === 0">
          <td colspan="6" style="text-align: center;">No hay productos en esta categoría.</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';
import ProductItem from './ProductItem.vue';

const props = defineProps({
  products: {
    type: Array,
    required: true
  }
});

defineEmits(['delete-product']);

const selectedCategory = ref('Todos');

const filteredProducts = computed(() => {
  if (selectedCategory.value === 'Todos') {
    return props.products;
  }
  return props.products.filter(p => p.categoria === selectedCategory.value);
});

const totalProductsCount = computed(() => {
  return props.products.length;
});

const totalInventoryValue = computed(() => {
  return props.products.reduce((total, p) => total + (p.precio * p.stock), 0);
});
</script>

<style scoped>
.list-container {
  background: white;
  padding: 25px;
  border-radius: 12px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.05);
  border: 1px solid #e2e8f0;
}

.summary-cards {
  display: flex;
  gap: 20px;
  margin-bottom: 25px;
}

.card {
  flex: 1;
  background: #f8fafc;
  padding: 15px;
  border-radius: 10px;
  text-align: center;
  border: 1px solid #e2e8f0;
}

.card .number {
  font-size: 1.5rem;
  font-weight: 800;
  color: #3b82f6;
}

.product-table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 15px;
}

.product-table th {
  background-color: #f1f5f9;
  padding: 12px;
  text-align: left;
  border-bottom: 2px solid #e2e8f0;
  font-size: 0.8rem;
  color: #64748b;
}

.filter-group {
  margin-bottom: 20px;
  font-weight: bold;
}

.filter-group {
  margin: 20px 0;
  font-family: sans-serif;
  color: #334155; 
  font-size: 0.95rem;
}

select {
  padding: 6px 10px;
  border-radius: 6px;
  border: 1px solid #cbd5e1;
  background-color: #ffffff;
  color: #1e293b;
  cursor: pointer;
  outline: none;
  transition: all 0.2s ease;
}

select:hover {
  border-color: #94a3b8;
}

select:focus {
  border-color: #3b82f6;
  box-shadow: 0 0 0 2px rgba(59, 130, 246, 0.1);
}
</style>