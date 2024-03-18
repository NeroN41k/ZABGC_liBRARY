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
    const apiUrl = 'https://9f6b75bab8c0eb87.mokky.dev/cart';
    
    if (!item.isAdded) {
      const obj = {
        book_id: item.id
      };
      
      item.isAdded = true;
      bookCartItems.value.push(item);

      const { data } = await axios.post(apiUrl, obj);
      item.book_id = data.id;
    } else {
      item.isAdded = false;
      const index = bookCartItems.value.indexOf(item);
      bookCartItems.value.splice(index, 1);

      await axios.delete(`${apiUrl}/${item.book_id}`);
      item.book_id = null;
    }
  } catch (err) {
    console.log(err);
  }
}

const removeFromCart = async (item) => {
  try {
    item.isAdded = false
    const index = bookCartItems.value.indexOf(item)
    bookCartItems.value.splice(index, 1)
    await axios.delete(`https://9f6b75bab8c0eb87.mokky.dev/cart/${item.book_id}`)
    item.book_id = null
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
  removeFromCart
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
