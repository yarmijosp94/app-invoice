<script setup lang="ts">

import type { Producto } from '@/types';

import { ref } from 'vue';

const getProducts = async ():Promise<Producto[]> => {
  try {
    const response = await fetch('https://fakestoreapi.com/products');
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }
    return await response.json();
  } catch (error) {
    console.error('Error fetching products:', error);
    return [];
  }
};


const counter = ref(0);


const increment = () => {
  counter.value++;
};

const decrement = () => {
  counter.value--;
};




const productos = await getProducts();

</script>

<template>
    <UDashboardPanel id="products">
    <template #header>
      <UDashboardNavbar title="Productos" :ui="{ right: 'gap-3' }">
        <template #leading>
          <UDashboardSidebarCollapse />
        </template>

        <template #right>
              <UButton variant="outline" color="primary" size="sm">Agregar Producto</UButton>
        </template>
      </UDashboardNavbar>
    </template>

    <template #body>
      <UCard v-for="producto in productos" :key="producto.id" class="mb-4">
        <UCardHeader>
          <UCardTitle>{{ producto.title }}</UCardTitle>
          <UCardSubtitle>{{ producto.category }}</UCardSubtitle>
        </UCardHeader>
        <UCardBody>
          <p>{{ producto.description }}</p>
          <p class="text-lg font-bold">${{ producto.price }}</p>
          <p>Counter: {{ counter }}</p>
          <input type="number" v-model="counter" class="border p-1 rounded w-20" />
        </UCardBody>
        <UCardFooter>
          <UButton variant="outline" color="primary" size="sm" @click="increment">Incrementar</UButton>
          <UButton variant="outline" color="primary" size="sm" @click="decrement">Decrementar</UButton>
        </UCardFooter>
      </UCard>
    </template>
  </UDashboardPanel>  
</template>

<style scoped>

</style>