<script setup>
import CardItem from './CardItem.vue'
defineProps({
  sneakers: Array,
  isFavorites: Boolean
})
const emit = defineEmits(['addToFavorite', 'addToCart'])
</script>
<template>
  <div class="grid grid-cols-4 gap-5" v-auto-animate>
    <CardItem
      v-for="sneaker in sneakers"
      :key="sneaker.id"
      :id="sneaker.id"
      :title="sneaker.title"
      :imgUrl="sneaker.imgUrl"
      :price="sneaker.price"
      :onClickFavorite="isFavorites ? null : () => emit('addToFavorite', sneaker)"
      :onClickAdd="() => emit('addToCart', sneaker)"
      :isFavorite="sneaker.isFavorite"
      :isAdded="sneaker.isAdded"
    />
  </div>
</template>
