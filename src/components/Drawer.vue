<script setup>
import DrawerHeader from './DrawerHeader.vue'
import CartItemList from './CartItemList.vue'
</script>

<template>
  <div class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-70"></div>
  <div class="bg-white w-96 h-full fixed right-0 top-0 z-20 p-8">
    <DrawerHeader />

    <CartItemList />

    <div class="flex flex-col gap-4 mt-7">
      <div class="flex gap-2">
        <span>Итого:</span>
        <div class="flex-1 border-b border-dashed"></div>
        <b>3 шт.</b>
      </div>

      <div class="flex gap-2">
        <span>Дата возврата:</span>
        <div class="flex-1 border-b border-dashed"></div>
        <b>11.08.2024</b>
      </div>

      <button
        :disabled="false"
        class="mt-4 transition bg-lime-500 w-full rounded-xl py-3 disabled:bg-slate-300 text-white hover:bg-lime-600 active:bg-lime-700 cursor-pointer"
      >
        Оформить
      </button>
    </div>
  </div>
</template>
