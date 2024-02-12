<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'

import CardList from '../components/CardList.vue'

const API_KEY = import.meta.env.VITE_MOKKY_API_KEY
const favorites = ref([])

onMounted(async () => {
  try {
    const { data } = await axios.get(`${API_KEY}/favorites?_relations=items`)

    favorites.value = data.map((obj) => obj.item)
  } catch (err) {
    console.log(err)
  }
})
</script>
<template>
  <h2 class="text-3xl font-bold mb-8">Favorites</h2>

  <CardList :sneakers="favorites" is-favorites />
</template>
