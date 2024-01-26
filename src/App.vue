<script setup>
import ShoppingCartVue from './components/ShoppingCart.vue'
import HomePage from './pages/HomePage.vue'
import HeaderItem from './components/HeaderItem.vue'
</script>

<template>
  <!-- <ShoppingCartVue /> -->
  <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14">
    <HeaderItem />
    <div class="p-10">
      <HomePage />
    </div>
  </div>
</template>

<style scoped></style>
