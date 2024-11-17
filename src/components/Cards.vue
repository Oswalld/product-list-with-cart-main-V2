<script setup>
import { defineProps, defineEmits, computed } from "vue";

const props = defineProps({
  item: {
    type: Object,
    required: true,
  },
  cart: {
    type: Array,
    required: true,
  },
});

const emits = defineEmits(["add-to-cart", "increment-item", "decrement-item"]);
const addToCart = () => {
  emits("add-to-cart", props.item);
};

const incrementItem = () => {
  emits("increment-item", props.item);
};

const decrementItem = () => {
  emits("decrement-item", props.item);
};

const quantity = computed(() => {
  const cartItem = props.cart.find((i) => i.name === props.item.name);
  return cartItem ? cartItem.quantity : 0;
});
</script>

<template>
  <div class="space-y-9">
    <picture class="relative">
      <!-- Image pour les écrans desktop (largeur minimum de 1024px) -->
      <source :srcset="item.image.desktop" media="(min-width: 1024px)" />

      <!-- Image pour les écrans tablette (largeur minimum de 768px) -->
      <source :srcset="item.image.tablet" media="(min-width: 768px)" />

      <!-- Image pour les écrans mobile (par défaut) -->
      <img class="rounded-lg" :srcset="item.image.mobile" :alt="item.name" />
      <button
        id="button-add-to-cart"
        @click="addToCart"
        :class="{ hidden: item.inCart }"
        class="flex items-center gap-2 absolute left-1/2 -translate-x-1/2 -bottom-6 font-semibold text-orange-950 bg-white text-primary rounded-full px-8 py-3 w-[180px] hover:text-orange-600 border-[1px] hover:border-orange-600 border-orange-800 cursor-pointer"
      >
        <svg
          xmlns="http://www.w3.org/2000/svg"
          width="21"
          height="20"
          fill="none"
          viewBox="0 0 21 20"
        >
          <g fill="#C73B0F" clip-path="url(#a)">
            <path
              d="M6.583 18.75a1.25 1.25 0 1 0 0-2.5 1.25 1.25 0 0 0 0 2.5ZM15.334 18.75a1.25 1.25 0 1 0 0-2.5 1.25 1.25 0 0 0 0 2.5ZM3.446 1.752a.625.625 0 0 0-.613-.502h-2.5V2.5h1.988l2.4 11.998a.625.625 0 0 0 .612.502h11.25v-1.25H5.847l-.5-2.5h11.238a.625.625 0 0 0 .61-.49l1.417-6.385h-1.28L16.083 10H5.096l-1.65-8.248Z"
            />
            <path
              d="M11.584 3.75v-2.5h-1.25v2.5h-2.5V5h2.5v2.5h1.25V5h2.5V3.75h-2.5Z"
            />
          </g>
          <defs>
            <clipPath id="a">
              <path fill="#fff" d="M.333 0h20v20h-20z" />
            </clipPath>
          </defs>
        </svg>
        Add to cart
      </button>
      <button
        id="button-manage-quantity"
        :class="{ hidden: !item.inCart }"
        class="flex justify-between text-white items-center absolute left-1/2 -translate-x-1/2 -bottom-6 font-semibold bg-orange-600 text-primary rounded-full py-3 w-[180px] border-[1px] border-orange-600"
      >
        <div
          id="minus"
          class="text-white border-[1px] border-white p-1 rounded-full flex items-center justify-center h-5 w-5 ml-4 hover:text-orange-600 hover:bg-white cursor-pointer"
          @click="decrementItem"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            width="10"
            height="2"
            fill="none"
            viewBox="0 0 10 2"
          >
            <path fill="CurrentColor" d="M0 .375h10v1.25H0V.375Z" />
          </svg>
        </div>
        <div>{{ quantity }}</div>
        <div
          id="plus"
          class="text-white border-[1px] border-white p-1 rounded-full flex items-center justify-center h-5 w-5 mr-4 hover:text-orange-600 hover:bg-white cursor-pointer"
          @click="incrementItem"
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
              d="M10 4.375H5.625V0h-1.25v4.375H0v1.25h4.375V10h1.25V5.625H10v-1.25Z"
            />
          </svg>
        </div>
      </button>
    </picture>
    <div>
      <div class="text-orange-900/70">{{ item.category }}</div>
      <h2 class="text-xl font-bold text-neutral-900">{{ item.name }}</h2>
      <div class="text-orange-700 font-bold">${{ item.price.toFixed(2) }}</div>
    </div>
  </div>
</template>
