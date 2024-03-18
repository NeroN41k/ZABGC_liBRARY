<script setup>
import axios from 'axios'
import { inject, reactive, watch, ref, onMounted } from 'vue'
import debounce from 'lodash.debounce'

import CardList from '../components/CardList.vue'

const { bookCartItems, addToCart } = inject('drawer')

const items = ref([])

const filters = reactive({
  sortBy: 'title',
  searchQuery: ''
})

//const onClickAddPlus = (item) => {
//if (!item.isAdded) {
// addToCart(item)
// } else {
// removeFromCart(item)
//}
//console.log(bookCartItems)
//}

const onChangeSelect = (event) => {
  filters.sortBy = event.target.value
}

const onChangeSearchInput = debounce((event) => {
  filters.searchQuery = event.target.value
}, 350)

const addToFavorite = async (item) => {
  try {
    const obj = {
      book_id: item.id
    };
    item.isFavorite = !item.isFavorite;
    
    if (item.isFavorite) {
      const { data } = await axios.post(`https://9f6b75bab8c0eb87.mokky.dev/favorites`, obj);
      item.favoriteId = data.id;
    } else {
      await axios.delete(`https://9f6b75bab8c0eb87.mokky.dev/favorites/${item.favoriteId}`);
      item.favoriteId = null;
    }
  } catch (err) {
    console.log(err);
  }
};

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

const fetchItems = async () => {
  try {
    const params = {
      sortBy: filters.sortBy
    }

    if (filters.searchQuery) {
      params.title = `*${filters.searchQuery}*`
    }

    const [booksRes, favoritesRes, cartRes] = await Promise.all([
      axios.get(`https://9f6b75bab8c0eb87.mokky.dev/books`, { params }),
      axios.get(`https://9f6b75bab8c0eb87.mokky.dev/favorites`),
      axios.get(`https://9f6b75bab8c0eb87.mokky.dev/cart`)
    ])

    const data = booksRes.data
    const favorites = favoritesRes.data
    const cart = cartRes.data

    items.value = data.map((obj) => ({
      ...obj,
      isFavorite: favorites && favorites.map((f) => f.bookIdFav).includes(obj.id),
      favoriteId: favorites ? obj.id : null,
      isAdded: cart && cart.map((f) => f.bookIdAdded).includes(obj.id),
      bookIdAdded: cart ? obj.id : null
    }))
  } catch (err) {
    console.log(err)
  }
}

onMounted(async () => {
  const localCart = localStorage.getItem('bookCartItems')
  bookCartItems.value = localCart ? JSON.parse(localCart) : []

  await fetchItems()
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
