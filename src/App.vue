<script setup>
import CardsResults from "./components/CardsResults.vue";
import Cards from "./components/Cards.vue";
import { ref, onMounted } from "vue";

const data = ref([]);
const cart = ref([]);

onMounted(async () => {
  try {
    const response = await fetch("./data/data.json");
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }
    const jsonData = await response.json();
    // Initialiser inCart à false pour tous les items
    data.value = jsonData.map((item) => ({ ...item, inCart: false }));
  } catch (error) {
    console.error("Failed to fetch data:", error);
  }
});

const addToCart = (item) => {
  const existingItem = cart.value.find(
    (cartItem) => cartItem.name === item.name
  );
  if (!existingItem) {
    // Mettre à jour l'item dans data.value
    const originalItem = data.value.find((i) => i.name === item.name);
    if (originalItem) {
      originalItem.inCart = true;
    }
    // Ajouter au panier
    cart.value.push({ ...item, quantity: 1, inCart: true });
  }
};

const incrementItem = (item) => {
  const cartItem = cart.value.find((i) => i.name === item.name);
  if (cartItem) {
    cartItem.quantity++;
  }
};

const decrementItem = (item) => {
  const cartItem = cart.value.find((i) => i.name === item.name);
  if (cartItem) {
    if (cartItem.quantity > 1) {
      cartItem.quantity--;
    } else {
      cart.value = cart.value.filter((i) => i.name !== item.name);
      // Mettre à jour l'état inCart dans data.value
      const originalItem = data.value.find((i) => i.name === item.name);
      if (originalItem) {
        originalItem.inCart = false;
      }
    }
  }
};

const deleteItem = (item) => {
  // Supprimer l'item du panier
  cart.value = cart.value.filter((i) => i.name !== item.name);

  // Mettre à jour l'état inCart dans data.value
  const originalItem = data.value.find((i) => i.name === item.name);
  if (originalItem) {
    originalItem.inCart = false;
  }
};
</script>

<template>
  <div
    class="w-full h-full p-4 lg:p-8 bg-[#fcf8f5] md:flex md:items-center md:justify-center"
  >
    <div class="lg:max-w-[1500px]">
      <h1 class="text-5xl text-orange-950 font-bold mb-8">Desserts</h1>
      <div class="flex flex-wrap gap-7 lg:flex-nowrap">
        <div class="md:grid lg:grid-cols-3 md:grid-cols-2 md:gap-7">
          <Cards
            v-for="item in data"
            :key="item.name"
            :item="item"
            :cart="cart"
            @add-to-cart="addToCart"
            @increment-item="incrementItem"
            @decrement-item="decrementItem"
            @delete-item="deleteItem"
          />
        </div>
        <CardsResults
          class="lg:max-w-[350px] lg:h-fit lg:-mt-16"
          :cart="cart"
          @delete-item="deleteItem"
        />
      </div>
    </div>
  </div>
</template>
