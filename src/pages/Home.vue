<script setup>
import axios from 'axios'
import { inject, reactive, watch, ref, onMounted, provide } from 'vue'
import debounce from 'lodash.debounce'

import CardList from '../components/CardList.vue'

const { bookCartItems, addToCart, addToFavorite } = inject('drawer')

const items = ref([])

const filters = reactive({
  sortBy: 'title',
  searchQuery: ''
})

const onChangeSelect = (event) => {
  filters.sortBy = event.target.value
}

const onChangeSearchInput = debounce((event) => {
  filters.searchQuery = event.target.value
}, 350)

const fetchFavorites = async () => {
  try {
    const response = await axios.get(`https://9f6b75bab8c0eb87.mokky.dev/favorites`)
    const favorites = response.data

    items.value = items.value.map((item) => {
      const favorite = favorites.find((favorite) => favorite.book_id === item.id)

      if (!favorite) {
        return item
      }

      return {
        ...item,
        isFavorite: true,
        favoriteId: favorite.id
      }
    })
  } catch (error) {
    console.log(error)
  }
}

const fetchCart = async () => {
  try {
    const response = await axios.get(`https://9f6b75bab8c0eb87.mokky.dev/cart`)
    const carts = response.data

    items.value = items.value.map((item) => {
      const cart = carts.find((cart) => cart.book_id === item.id)

      if (!cart) {
        return item
      }

      return {
        ...item,
        isAdded: true,
        cartId: cart.id
      }
    })
  } catch (err) {
    console.log(err)
  }
}

const fetchItems = async () => {
  try {
    const params = {
      sortBy: filters.sortBy
    }

    const [booksRes, favoritesRes, cartRes] = await Promise.all([
      axios.get(`https://9f6b75bab8c0eb87.mokky.dev/books`, { params }),
      axios.get(`https://9f6b75bab8c0eb87.mokky.dev/favorites`),
      axios.get(`https://9f6b75bab8c0eb87.mokky.dev/cart`)
    ])

    const data = filters.searchQuery
      ? booksRes.data.filter((x) => searchFilter(x, filters.searchQuery))
      : booksRes.data

    const favorites = favoritesRes.data
    const cart = cartRes.data

    items.value = data.map((obj) => ({
      ...obj,
      isFavorite: favorites && favorites.map((f) => f.book_id).includes(obj.id),
      favoriteId: favorites ? obj.id : null,
      isAdded: cart && cart.map((f) => f.book_id).includes(obj.id),
      cartId: cart ? obj.id : null
    }))
  } catch (err) {
    console.log(err)
  }
}

const searchFilter = (book, query) => {
  return book.title.toLowerCase().includes(query) || book.author.toLowerCase().includes(query)
}

onMounted(async () => {
  const localCart = localStorage.getItem('bookCartItems')
  bookCartItems.value = localCart ? JSON.parse(localCart) : []

  await fetchItems()
  await fetchCart()
  await fetchFavorites()

  items.value = items.value.map((item) => ({
    ...item,
    isAdded: bookCartItems.value.some((cartBook) => cartBook.id === item.id)
  }))
})

watch(bookCartItems, () => {
  items.value = items.value.map((item) => ({
    ...item,
    isAdded: false
  }))
})
watch(filters, fetchItems)

provide('aboba', {
  fetchItems,
  fetchFavorites,
  fetchCart
})
</script>

<template>
  <div class="flex justify-between items-center">
    <h2 class="text-3xl font-bold mb-8">Все книги</h2>

    <div class="flex gap-4">
      <select @change="onChangeSelect" class="py-2 px-3 border rounded-md outline-none">
        <option value="name">По названию</option>
      </select>

      <div class="relative">
        <img class="absolute left-3 top-3" src="/search.svg" alt="Search" />
        <input
          @input="onChangeSearchInput"
          class="border rounded-md py-2 pl-11 pr-4 outline-none focus:border-gray-400"
          type="text"
          placeholder="Поиск..."
        />
      </div>
    </div>
  </div>

  <div class="mt-10">
    <CardList :items="items" @add-to-favorite="addToFavorite" @add-to-cart="addToCart" />
  </div>
</template>
