<script setup>
import { inject } from 'vue'
import CartItem from './CartItem.vue'
const { cartItems, deleteFromCart } = inject('cart')
</script>

<template>
  <div class="flex flex-col flex-1 gap-4 justify-between" v-auto-animate>
    <CartItem
      v-for="item in cartItems"
      :key="item.id"
      :title="item.title"
      :price="item.price"
      :imgUrl="item.imgUrl"
      @on-click-remove="() => deleteFromCart(item)"
    />
  </div>
</template>
