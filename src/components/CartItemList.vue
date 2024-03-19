<script setup>
import { inject } from 'vue'
import CartItem from './CartItem.vue'

const { bookCartItems } = inject('drawer')
</script>

<template>
  <div class="flex flex-col flex-1 gap-4 justify-between" v-auto-animate>
    <CartItem
      v-for="item in bookCartItems"
      :key="item.id"
      :item="item"
    />
  </div>
</template>
