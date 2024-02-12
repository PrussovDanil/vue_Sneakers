<script setup>
import { inject } from 'vue'
defineProps({
  totalPrice: Number
})

const { openCart } = inject('cart')
</script>
<template>
  <header class="flex justify-between border-b border-slate-200 px-10 py-8">
    <router-link to="/">
      <div class="flex items-center gap-4">
        <img src="/logo.png" alt="Logo" class="w-11" />
        <div>
          <h2 class="font-bold text-xl uppercase">Vue Sneakers</h2>
          <p class="text-slate-400">Best Shop</p>
        </div>
      </div>
    </router-link>
    <ul class="flex items-center gap-10">
      <li @click="openCart" class="flex items-center gap-3 cursor-pointer">
        <img src="/cart.svg" alt="Cart" />
        <b>{{ totalPrice }} $</b>
      </li>
      <router-link to="/favorites">
        <li class="flex items-center gap-3 cursor-pointer">
          <img src="/favorite.svg" alt="Favorite" />
          <span>Favorite</span>
        </li>
      </router-link>

      <li class="flex items-center gap-3 cursor-pointer">
        <img src="/profile.svg" alt="Profile" />
        <span>Profile</span>
      </li>
    </ul>
  </header>
</template>
