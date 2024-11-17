<script setup>
import { defineProps, computed, defineEmits } from "vue";

const props = defineProps({
  cart: {
    type: Array,
    required: true,
  },
});

const emit = defineEmits(["delete-item"]);

const deleteItem = (item) => {
  emit("delete-item", item);
};

// Calculer le total de la commande
const orderTotal = computed(() => {
  return props.cart.reduce((total, item) => {
    return total + item.price * item.quantity;
  }, 0);
});

const totalCartItem = computed(() => {
  return props.cart.reduce((total, item) => {
    return total + item.quantity;
  }, 0);
});
</script>

<template>
  <div class="bg-white rounded-xl w-full p-6">
    <div class="text-orange-700 font-bold text-xl">
      Your Cart(<span>{{ totalCartItem }}</span
      >)
    </div>

    <div
      v-if="cart.length === 0"
      class="flex flex-col items-center justify-center pt-5 space-y-3"
    >
      <img src="\images\illustration-empty-cart.svg" alt="empty cart" />
      <div class="text-orange-900 text-xs font-semibold">
        Your added items will appear here
      </div>
    </div>

    <template v-else>
      <div
        v-for="item in cart"
        :key="item.name"
        class="flex items-center justify-between border-b-[1px] border-orange-900/10 py-4"
      >
        <div class="space-y-1">
          <div class="font-semibold text-orange-900">{{ item.name }}</div>
          <div class="space-x-1">
            <span class="text-orange-700 font-semibold pr-2"
              >{{ item.quantity }}x</span
            >
            <span class="text-orange-900/50"
              ><span class="text-sm">@</span> ${{ item.price.toFixed(2) }}</span
            >
            <span class="text-orange-900/70 font-semibold"
              >${{ (item.price * item.quantity).toFixed(2) }}</span
            >
          </div>
        </div>
        <button
          @click="deleteItem(item)"
          class="text-orange-900/50 hover:text-orange-900 border-[1px] rounded-full p-[4px] border-orange-900/50 hover:border-orange-900"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            width="10"
            height="10"
            fill="none"
            viewBox="0 0 10 10"
          >
            <path
              fill="CurrentColor"
              d="M8.375 9.375 5 6 1.625 9.375l-1-1L4 5 .625 1.625l1-1L5 4 8.375.625l1 1L6 5l3.375 3.375-1 1Z"
            />
          </svg>
        </button>
      </div>

      <div class="flex items-center justify-between py-6">
        <div>Order Total</div>
        <div class="text-orange-900 text-2xl font-bold">
          ${{ orderTotal.toFixed(2) }}
        </div>
      </div>

      <div
        class="flex items-center justify-center gap-2 p-3 bg-amber-900/5 rounded-lg"
      >
        <svg
          xmlns="http://www.w3.org/2000/svg"
          width="21"
          height="20"
          fill="none"
          viewBox="0 0 21 20"
        >
          <path
            fill="#1EA575"
            d="M8 18.75H6.125V17.5H8V9.729L5.803 8.41l.644-1.072 2.196 1.318a1.256 1.256 0 0 1 .607 1.072V17.5A1.25 1.25 0 0 1 8 18.75Z"
          />
          <path
            fill="#1EA575"
            d="M14.25 18.75h-1.875a1.25 1.25 0 0 1-1.25-1.25v-6.875h3.75a2.498 2.498 0 0 0 2.488-2.747 2.594 2.594 0 0 0-2.622-2.253h-.99l-.11-.487C13.283 3.56 11.769 2.5 9.875 2.5a3.762 3.762 0 0 0-3.4 2.179l-.194.417-.54-.072A1.876 1.876 0 0 0 5.5 5a2.5 2.5 0 1 0 0 5v1.25a3.75 3.75 0 0 1 0-7.5h.05a5.019 5.019 0 0 1 4.325-2.5c2.3 0 4.182 1.236 4.845 3.125h.02a3.852 3.852 0 0 1 3.868 3.384 3.75 3.75 0 0 1-3.733 4.116h-2.5V17.5h1.875v1.25Z"
          />
        </svg>
        <p class="text-xs text-orange-900">
          This is a
          <span class="font-bold">carbon-neutral</span> delivery
        </p>
      </div>

      <button
        class="rounded-full w-full bg-orange-600 text-white font-semibold p-3 mt-6 hover:bg-orange-950"
      >
        Confirm order
      </button>
    </template>
  </div>
</template>
