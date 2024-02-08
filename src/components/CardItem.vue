<script setup>
defineProps({
  id: Number,
  imgUrl: String,
  title: String,
  price: Number,
  isFavorite: Boolean,
  isAdded: Boolean,
  onClickFavorite: Function,
  onClickAdd: Function
})
</script>

<template>
  <div
    class="z-0 relative bg-white border border-slate-100 rounded-3xl p-8 cursor-pointer transition hover:-translate-y-2 hover:shadow-xl"
  >
    <img
      v-if="onClickFavorite"
      :src="!isFavorite ? '/like-1.svg' : '/like-2.svg'"
      alt="like 1"
      @click="onClickFavorite"
      class="absolute top-8 left-8 transition hover:-translate-y-0.5 hover:shadow-xl"
    />

    <img :src="imgUrl" alt="Sneaker" class="w-full" />

    <p>{{ title }}</p>
    <div class="flex justify-between mt-5">
      <div class="flex flex-col gap-2">
        <span class="text-slate-400">Price:</span>
        <span class="font-bold"> {{ price }} USD</span>
      </div>
      <div>
        <img
          :src="isAdded ? '/checked.svg' : '/plus.svg'"
          alt="Plus"
          @click="onClickAdd"
          class="transition hover:shadow-xl hover:-translate-y-0.5"
        />
      </div>
    </div>
  </div>
</template>
