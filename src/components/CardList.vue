<script setup>
import CardItem from './CardItem.vue'
defineProps({
  sneakers: Array,
  isFavorites: Boolean
})

const emit = defineEmits(['addToFavorite'])
</script>
<template>
  <div class="grid grid-cols-4 gap-5">
    <CardItem
      v-for="sneaker in sneakers"
      :key="sneaker.id"
      :id="sneaker.id"
      :title="sneaker.title"
      :imgUrl="sneaker.imgUrl"
      :price="sneaker.price"
      :onClickFavorite="isFavorites ? null : () => emit('addToFavorite', sneaker)"
      :isFavorite="sneaker.isFavorite"
    />
  </div>
</template>
