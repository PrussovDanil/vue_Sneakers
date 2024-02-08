<script setup>
import ShoppingCart from './components/ShoppingCart.vue'
import HomePage from './pages/HomePage.vue'
import HeaderItem from './components/HeaderItem.vue'
import ShoppingCartHead from './components/ShoppingCartHead.vue'
import CartItemList from './components/CartItemList.vue'
import { provide, ref } from 'vue'
const shoppingCartOpen = ref(false)
const closeCart = () => {
  shoppingCartOpen.value = false
}
const openCart = () => {
  shoppingCartOpen.value = true
}
provide('cart', { closeCart, openCart })
</script>

<template>
  <ShoppingCart :shopping-cart-open="shoppingCartOpen" />
  <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14">
    <HeaderItem />
    <div class="p-10">
      <HomePage />
    </div>
  </div>
</template>
