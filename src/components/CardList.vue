<script setup>
import { inject } from 'vue'
import Card from './Card.vue'

const { addToCart } = inject('drawer')

defineProps({
  items: Array,
  isFavorites: Boolean
})

const emit = defineEmits(['addToFavorite'])
</script>

<template>
  <div class="grid grid-cols-4 gap-5" v-auto-animate>
    <Card
      v-for="item in items"
      :key="item.id"
      :id="item.id"
      :author="item.author"
      :title="item.title"
      :imageUrl="item.imageUrl"
      :count="item.count"
      :onClickFavorite="isFavorites ? null : () => emit('addToFavorite', item)"
      :onClickAdd="isFavorites ? null : () => addToCart(item)"
      :isFavorite="item.isFavorite"
      :isAdded="item.isAdded"
    />
  </div>
</template>