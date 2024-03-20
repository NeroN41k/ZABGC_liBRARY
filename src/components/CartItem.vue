<script setup>
import { inject } from 'vue'
import { vueScrollbar } from 'vue-custom-scrollbar';
import "vue-custom-scrollbar/dist/vueScrollbar.css"

const { addToCart } = inject('drawer')

const props = defineProps({
  item: {
    id: Number,
    title: String,
    author: String,
    imageUrl: String
  }
})
</script>

<template>
  <div class="flex border border-slate-200 p-4 rounded-xl gap-4">
    <vueCustomScrollbar class="scroll-area" :settings="settings" @ps-scroll-y="scrollHandle">
      <img class="w-16 h-16" :src="props.item.imageUrl" :alt="props.item.title" />

      <div class="flex flex-col flex-1">
        <p>{{ props.item.title }}</p>

        <div class="flex justify-between mt-2">
          <p class="text-slate-400 flex-1">{{ props.item.author }}</p>
          <img
            @click="addToCart(item)"
            class="opacity-40 hover:opacity-100 cursor-pointer transition ml-1 w-8 h-8"
            src="/close.svg"
          />
        </div>
      </div>
    </vueCustomScrollbar>
  </div>
</template>