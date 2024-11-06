<script setup>
import Pinia_Logo from "../assets/svg/Pinia.svg";
import { ShoppingCartIcon, MagnifyingGlassIcon } from "@heroicons/vue/24/solid";
import { ref, computed } from "vue";
import { useProductStore } from "../stores/productStore";

const search = ref("");
const productStore = useProductStore();
const searchProducts = computed(() => {
  if (!search.value || search.value.length < 2) return [];

  return productStore.products.filter((item) => {
    return item.title.toLowerCase().includes(search.value.toLowerCase());
  });
});

const handleSubmit = () => {
  // Functionality for handling search submission can go here
};
</script>

<template>
  <div>
    <header>
      <nav
        class="w-full h-16 flex justify-between items-center p-4 bg-gray-100 shadow-md"
      >
        <div class="flex items-center space-x-2">
          <img :src="Pinia_Logo" alt="Pinia" class="w-10 h-10" />
          <h1 class="font-bold text-lg md:text-2xl">
            <router-link to="/">Store</router-link>
          </h1>
        </div>

        <div class="hidden md:block relative">
          <div class="relative">
            <div
              class="flex absolute inset-y-0 left-0 items-center pl-3 pointer-events-none"
            >
              <MagnifyingGlassIcon class="w-5 h-5 text-gray-500" />
            </div>
            <input
              type="search"
              v-model="search"
              @keyup.enter="handleSubmit"
              class="block p-3 pl-10 w-80 text-sm text-gray-800 bg-white rounded-lg border border-gray-300 focus:outline-none focus:ring-2 focus:ring-blue-500"
              placeholder="Search Products..."
              required
            />
          </div>

          <!-- Dropdown results -->
          <div
            v-if="searchProducts.length"
            class="absolute bg-white shadow-lg rounded-lg mt-1 w-80 z-10 max-h-48 overflow-y-auto border border-gray-300"
          >
            <ul>
              <li
                v-for="product in searchProducts"
                :key="product.id"
                class="p-3 hover:bg-gray-100 cursor-pointer border-b border-gray-200 last:border-b-0"
              >
                <router-link
                  :to="{ name: 'product', params: { id: product.id } }"
                  class="block text-gray-700"
                >
                  {{ product.title }}
                </router-link>
              </li>
            </ul>
          </div>
        </div>

        <div class="relative flex items-center">
          <router-link to="/cart" class="relative">
            <div
              class="absolute top-0 right-0 w-5 h-5 rounded-full bg-red-500 flex items-center justify-center text-white text-xs font-semibold"
            >
              {{ productStore.cartQuantity }}
            </div>
            <ShoppingCartIcon class="w-6 h-6 md:w-8 md:h-8 text-gray-700" />
          </router-link>
        </div>
      </nav>
    </header>
  </div>
</template>
