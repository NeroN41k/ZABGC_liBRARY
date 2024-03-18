<script setup>
import { inject } from 'vue'

const { removeFromCart } = inject('drawer')

const props = defineProps({
  id: Number,
  title: String,
  author: String,
  imageUrl: String
})
</script>

<template>
  <div class="flex border border-slate-200 p-4 rounded-xl gap-4">
    <img class="w-16 h-16" :src="props.imageUrl" :alt="props.title" />

    <div class="flex flex-col flex-1">
      <p>{{ props.title }}</p>

      <div class="flex justify-between mt-2">
        <p class="text-slate-400 flex-1">{{ props.author }}</p>
        <img
          @click="removeFromCart(item)"
          class="opacity-40 hover:opacity-100 cursor-pointer transition ml-1 w-8 h-8"
          src="/close.svg"
        />
      </div>
    </div>
  </div>
</template>