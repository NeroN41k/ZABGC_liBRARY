<script setup>
import { onMounted, ref } from 'vue'
import axios from 'axios'

import CardList from '../components/CardList.vue'

const favorites = ref([])

onMounted(async () => {
  try {
    const { data } = await axios.get(
      `https://9f6b75bab8c0eb87.mokky.dev/favorites?_relations=books`
    )
    favorites.value = data.map((obj) => obj.book)
  } catch (err) {
    console.log(err)
  }
})
</script>

<template>
  <h2 class="text-3xl font-bold mb-8">Мои закладки</h2>

  <CardList :items="favorites" is-favorites />
</template>