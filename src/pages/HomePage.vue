<script setup>
import { onMounted, reactive, ref, watch } from 'vue'
import CardList from '../components/CardList.vue'
import axios from 'axios'
import debounce from 'lodash.debounce'
import { inject } from 'vue'
const API_KEY = import.meta.env.VITE_MOKKY_API_KEY
const sneakers = ref([])
const filters = reactive({
  sortBy: 'title',
  searchQuery: ''
})

const { addToCart, deleteFromCart } = inject('cart')

const onClickAddPlus = (item) => {
  if (!item.isAdded) {
    addToCart(item)
  } else {
    deleteFromCart(item)
  }
}

const onChangeSelect = (event) => {
  filters.sortBy = event.target.value
}
const onChangeSearchInput = debounce((event) => {
  filters.searchQuery = event.target.value
}, 300)

const addToFavorite = async (item) => {
  try {
    if (!item.isFavorite) {
      const obj = {
        item_id: item.id
      }

      item.isFavorite = true

      const { data } = await axios.post(`${API_KEY}/favorites`, obj)

      item.favoriteId = data.id
    } else {
      item.isFavorite = false
      await axios.delete(`${API_KEY}/favorites/${item.favoriteId}`)
      item.favoriteId = null
    }
  } catch (err) {
    console.log(err)
  }
}

const fetchFavorites = async () => {
  try {
    const { data: favorites } = await axios.get(`${API_KEY}/favorites`)

    sneakers.value = sneakers.value.map((item) => {
      const favorite = favorites.find((favorite) => favorite.item_id === item.id)

      if (!favorite) {
        return item
      }
      return {
        ...item,
        isFavorite: true,
        favoriteId: favorite.id
      }
    })
  } catch (err) {
    console.log(err)
  }
}

const fetchSneakers = async () => {
  try {
    const params = {
      sortBy: filters.sortBy
    }

    if (filters.searchQuery) {
      params.title = `*${filters.searchQuery}*`
    }
    const { data } = await axios.get(`${API_KEY}/items`, {
      params
    })
    sneakers.value = data.map((obj) => ({
      ...obj,
      isFavorite: false,
      isAdded: false
    }))
  } catch (error) {
    console.log(error)
  }
}
onMounted(async () => {
  await fetchSneakers()
  await fetchFavorites()
})
watch(filters, fetchSneakers)
</script>
<template>
  <div class="flex justify-between items-center">
    <h2 class="text-3xl font-bold mb-8">All Sneakers</h2>
    <div class="flex gap-4">
      <select @change="onChangeSelect" class="py-2 px-3 border rounded-md outline-none">
        <option value="name">By name</option>
        <option value="price">By price (cheap)</option>
        <option value="-price">By price (expensive)</option>
      </select>
      <div class="relative">
        <img class="absolute left-4 top-3" src="/search.svg" />
        <input
          @input="onChangeSearchInput"
          class="border rounded-md py-2 pl-11 pr-4 outline-none focus:border-gray-400"
          type="text"
          placeholder="Search..."
        />
      </div>
    </div>
  </div>
  <div class="mt-10">
    <CardList :sneakers="sneakers" @add-to-favorite="addToFavorite" @add-to-cart="onClickAddPlus" />
  </div>
</template>
