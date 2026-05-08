<template>
  <div class="form-container">
    <h2>Crear Producto</h2>
    
    <form @submit.prevent="handleSubmit">
      <div class="form-group">
        <label for="nombre">Nombre del producto:</label>
        <input 
          id="nombre" 
          type="text" 
          v-model.trim="newProduct.nombre" 
          required 
          placeholder="Ej: Teclado" 
        />
      </div>

      <div class="form-group">
        <label for="precio">Precio:</label>
        <input 
          id="precio" 
          type="number" 
          v-model.number="newProduct.precio" 
          required 
          min="0.01" 
          step="any" 
          placeholder="0" 
        />
      </div>

      <div class="form-group">
        <label for="stock">Stock:</label>
        <input 
          id="stock" 
          type="number" 
          v-model.number="newProduct.stock" 
          required 
          min="0" 
          step="1" 
          placeholder="0" 
        />
      </div>

      <div class="form-group">
        <label for="categoria">Categoría:</label>
        <select id="categoria" v-model="newProduct.categoria" required>
          <option disabled value="">Seleccione una categoría</option>
          <option value="Electrónica">Electrónica</option>
          <option value="Hardware">Hardware</option>
          <option value="Accesorios">Accesorios</option>
        </select>
      </div>

      <button type="submit">Guardar Producto</button>
      
      <p v-if="errorMessage" class="error-message">{{ errorMessage }}</p>
    </form>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const emit = defineEmits(['add-product']);

const newProduct = ref({
  nombre: '',
  precio: null,
  stock: null,
  categoria: ''
});

const errorMessage = ref('');

const handleSubmit = () => {
  const { nombre, precio, stock, categoria } = newProduct.value;

  if (!nombre || precio === null || stock === null || !categoria) {
    errorMessage.value = '⚠️ Todos los campos son obligatorios.';
    return;
  }

  if (typeof precio !== 'number' || precio <= 0) {
    errorMessage.value = '⚠️ El precio debe ser mayor a 0.';
    return;
  }

  if (typeof stock !== 'number' || stock < 0) {
    errorMessage.value = '⚠️ El stock no puede ser un número negativo.';
    return;
  }

  emit('add-product', { ...newProduct.value });

  newProduct.value = { nombre: '', precio: null, stock: null, categoria: '' };
  errorMessage.value = '';
};
</script>

<style scoped>
.form-container {
  background: white;
  padding: 20px;
  border-radius: 12px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.05);
  border: 1px solid #e2e8f0;
}

form {
  display: grid;
  grid-template-columns: 1fr 1fr; 
  gap: 15px;
}

input, select {
  padding: 10px;
  border: 1px solid #cbd5e1;
  border-radius: 6px;
  font-size: 14px;
}

button {
  grid-column: span 2;
  background-color: #3b82f6;
  color: white;
  padding: 12px;
  border: none;
  border-radius: 6px;
  font-weight: bold;
  cursor: pointer;
  transition: background 0.3s;
}

button:hover {
  background-color: #059669;
}

label {
  display: inline-block; 
  width: 160px;    
  text-align: right; 
  margin-right: 10px;
}
</style>