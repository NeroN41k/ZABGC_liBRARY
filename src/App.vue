<script setup>
import { ref, provide, watch, computed } from 'vue'
import Header from './components/Header.vue'
import Drawer from './components/Drawer.vue'
import axios from 'axios'

/* Cart */

const bookCartItems = ref([])
const drawerOpen = ref(false)

const totalCountCart = computed(() => bookCartItems.value.length)

const openDrawer = () => {
  drawerOpen.value = true
}
const closeDrawer = () => {
  drawerOpen.value = false
}

const addToCart = async (item) => {
  try {
    const apiUrl = `https://9f6b75bab8c0eb87.mokky.dev/cart`
    const obj = {
      book_id: item.id
    }

    if (!item.isAdded) {
      const { data } = await axios.post(apiUrl, obj)
      item.isAdded = true
      item.cartId = data.id
      bookCartItems.value.push(item)
    } else {
      await axios.delete(`${apiUrl}/${item.cartId}`)
      item.isAdded = false
      item.cartId = null
      const index = bookCartItems.value.indexOf(item)
      bookCartItems.value.splice(index, 1)
    }
  } catch (err) {
    console.log(err)
  }
}

const addToFavorite = async (item) => {
  try {
    const obj = {
      book_id: item.id
    }
    item.isFavorite = !item.isFavorite

    if (item.isFavorite) {
      const { data } = await axios.post(`https://9f6b75bab8c0eb87.mokky.dev/favorites`, obj)
      item.favoriteId = data.id
    } else {
      await axios.delete(`https://9f6b75bab8c0eb87.mokky.dev/favorites/${item.favoriteId}`)
      item.favoriteId = null
    }
  } catch (err) {
    console.log(err)
  }
}

watch(
  bookCartItems,
  () => {
    localStorage.setItem('bookCartItems', JSON.stringify(bookCartItems.value))
  },
  { deep: true }
)

provide('drawer', {
  bookCartItems,
  openDrawer,
  closeDrawer,
  addToCart,
  addToFavorite
})
/* Cart */
</script>

<template>
  <Drawer v-if="drawerOpen" :total-count-cart="totalCountCart" />
  <div class="bg-white w-3/5 m-auto rounded-xl shadow-xl shadow-grey-200 mt-10 mb-5">
    <Header :total-count-cart="totalCountCart" @open-drawer="openDrawer" />

    <div class="p-10">
      <router-view></router-view>
    </div>
  </div>
</template>
