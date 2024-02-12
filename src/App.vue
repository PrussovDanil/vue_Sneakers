<script setup>
import ShoppingCart from './components/ShoppingCart.vue'
import HomePage from './pages/HomePage.vue'
import HeaderItem from './components/HeaderItem.vue'
import { provide, computed, ref } from 'vue'

const shoppingCartOpen = ref(false)
const cartItems = ref([])

const totalPrice = computed(
  () => Math.round(cartItems.value.reduce((acc, item) => acc + item.price, 0) * 100) / 100
)
const vatPrice = computed(() => Math.round((totalPrice.value * 5) / 100))

const addToCart = (item) => {
  cartItems.value.push(item)
  item.isAdded = true
}

const deleteFromCart = (item) => {
  cartItems.value.splice(cartItems.value.indexOf(item), 1)
  item.isAdded = false
}

const closeCart = () => {
  shoppingCartOpen.value = false
}
const openCart = () => {
  shoppingCartOpen.value = true
}

provide('cart', { closeCart, openCart, addToCart, deleteFromCart, cartItems })
</script>

<template>
  <ShoppingCart
    :shopping-cart-open="shoppingCartOpen"
    :total-price="totalPrice"
    :vatPrice="vatPrice"
  />
  <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14">
    <HeaderItem :total-price="totalPrice" />
    <div class="p-10">
      <router-view></router-view>
    </div>
  </div>
</template>
