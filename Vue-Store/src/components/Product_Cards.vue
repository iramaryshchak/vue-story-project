<script setup>
import { useProductStore } from "../stores/productStore";
import Loader from "./Loader.vue";
import { ref, watchEffect } from "vue";
import { useToast } from "vue-toastification";
const toast = useToast();
const productStore = useProductStore();
const products = ref([]);
const loading = ref(null);
watchEffect(() => {
  productStore.fetchProducts();
});

watchEffect(() => {
  loading.value = productStore.loading;
  products.value = productStore.products;
});

const handleAddToCart = (product) => {
  productStore.addToCart(product);
  toast.success("Product added to cart successfully", {
    timeout: 3000,
  });
};
</script>
<template>
  <main class="flex-center flex-wrap gap-6 p-4">
    <div v-if="loading">
      <Loader />
    </div>
    <div
      v-else
      v-for="product in products"
      :key="product.id"
      class="bg-blue-100 shadow-lg border border-blue-300 rounded-lg w-80 m-4 p-5 transform transition-transform hover:scale-105 duration-300"
    >
      <div class="flex justify-center mb-4">
        <img
          :src="product.image"
          :alt="product.title"
          class="w-48 h-48 md:w-64 md:h-64 object-cover rounded-lg"
        />
      </div>
      <div class="space-y-3 text-center">
        <h1 class="text-xl font-semibold text-blue-900 line-clamp-1">
          {{ product.title }}
        </h1>
        <h4 class="text-lg font-bold text-blue-700">
          Price: ${{ product.price }}
        </h4>
        <button
          @click="handleAddToCart(product)"
          class="bg-yellow-400 hover:bg-yellow-300 text-black font-bold py-2 px-4 rounded-md transition duration-300 ease-in-out"
        >
          Add To Cart
        </button>
      </div>
    </div>
  </main>
</template>
