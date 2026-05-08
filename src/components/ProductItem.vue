<template>
  <tr>
    <td>{{ product.id }}</td>
    <td>{{ product.nombre }}</td>
    <td>{{ product.categoria }}</td>
    <td>${{ product.precio }}</td>
    <td>{{ product.stock }}</td>
    <td>
      <button class="delete-btn" @click="$emit('delete-product', product.id)">
        Eliminar
      </button>
    </td>
  </tr>
</template>

<script setup>
const props = defineProps({
  product: {
    type: Object,
    required: true
  }
});

defineEmits(['delete-product']);
</script>

<style scoped>
tr:hover {
  background-color: #f8fafc;
}

td {
  padding: 12px;
  border-bottom: 1px solid #f1f5f9;
}

.delete-btn {
  background-color: #ef4444;
  color: white;
  border: none;
  padding: 6px 12px;
  border-radius: 6px;
  cursor: pointer;
}

.delete-btn:hover {
  background-color: #b91c1c;
}
</style>