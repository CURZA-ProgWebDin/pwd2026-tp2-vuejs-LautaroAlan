<template>
  <div class="app-container">
    <header>
      <h1>Gestión de Inventario</h1>
    </header>
    
    <main class="main-content">
      <ProductForm @add-product="addProduct" />
      
      <ProductList 
        :products="products" 
        @delete-product="deleteProduct" 
      />
    </main>
  </div>
</template>

<script setup>
import { reactive, computed } from 'vue';
import ProductForm from './components/ProductForm.vue';
import ProductList from './components/ProductList.vue';

const products = reactive([
  { id: 1, nombre: 'Monitor Gamer 24"', precio: 250, stock: 10, categoria: 'Hardware' }
]);

const addProduct = (productData) => {
  const newProduct = {
    id: Date.now(),
    ...productData
  };
  products.push(newProduct);
};

const deleteProduct = (id) => {
  const index = products.findIndex(p => p.id === id);
  if (index !== -1) {
    products.splice(index, 1);
  }
};
</script>

<style>
.app-container {
  max-width: 900px;
  margin: 0 auto;
  padding: 20px;
  font-family: Arial, sans-serif;
}
.main-content {
  display: flex;
  flex-direction: column;
  gap: 20px;
}
</style>