<script setup>
defineProps({
  author: String,
  title: String,
  imageUrl: String,
  count: Number,
  isFavorite: Boolean,
  isAdded: Boolean,
  onClickAdd: Function,
  onClickFavorite: Function
})
</script>

<template>
  <div>
    <div
      class="relative bg-white border border-slate-100 rounded-xl p-8 cursor-pointer transition hover:-translate-y-2 hover:shadow-xl"
    >
      <img
        v-if="onClickFavorite"
        @click="onClickFavorite"
        :src="!isFavorite ? '/like-1.svg' : '/like-2.svg'"
        alt="Like 1"
        class="absolute top-5 left-5"
      />
      <img :src="imageUrl" alt="Book" />
      <p>{{ title }}</p>
      <p class="text-slate-400 mt-3">{{ author }}</p>

      <div class="flex justify-between mt-3">
        <div class="flex flex-col">
          <span class="text-slate-400">В наличии:</span>
          <b>{{ count }} шт.</b>
        </div>

        <img
          v-if="onClickAdd"
          @click="onClickAdd"
          :src="!isAdded ? '/plus.svg' : '/checked.svg'"
          alt="Plus"
        />
      </div>
    </div>
  </div>
</template>
