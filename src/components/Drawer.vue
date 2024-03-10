<script setup>
import axios from 'axios'
import { ref, watch, computed, inject } from 'vue'

import DrawerHeader from './DrawerHeader.vue'
import CartItemList from './CartItemList.vue'
import InfoBlock from './InfoBlock.vue'

const props = defineProps({
  totalCountCart: Number
})

const { bookCartItems, closeDrawer } = inject('drawer')

const isCreating = ref(false)
const orderId = ref(null)

const createOrder = async () => {
  try {
    isCreating.value = true
    const { data } = await axios.post(`https://9f6b75bab8c0eb87.mokky.dev/orders`, {
      books: bookCartItems.value,
      totalCountCart: props.totalCountCart.value
    })

    bookCartItems.value = []

    orderId.value = data.id
  } catch (err) {
    console.log(err)
  } finally {
    isCreating.value = false
  }
}

const cartIsEmpty = computed(() => bookCartItems.value.length === 0)
const buttonDisabled = computed(() => isCreating.value || cartIsEmpty.value)
</script>

<template>
  <div class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-70"></div>
  <div class="bg-white w-96 h-full fixed right-0 top-0 z-20 p-8">
    <DrawerHeader />

    <div v-if="!totalCountCart || orderId" class="flex h-full items-center">
      <InfoBlock
        v-if="!totalCountCart && !orderId"
        title="Корзина пуста"
        description="Добавьте хотя бы одну книгу, чтобы сделать заказ."
        image-url="/package-icon.png"
      />
      <InfoBlock
        v-if="orderId"
        title="Книга оформлена!"
        :description="`Ваш заказ №${orderId} подготавливается и в НИКОГДА вы сможете ее получить в библиотеке`"
        image-url="/order-success-icon.png"
      />
    </div>

    <div v-else>
      <CartItemList v-auto-animate />

      <div class="flex flex-col gap-4 mt-7">
        <div class="flex gap-2">
          <span>Итого:</span>
          <div class="flex-1 border-b border-dashed"></div>
          <b>{{ totalCountCart }} шт.</b>
        </div>

        <div class="flex gap-2">
          <span>Дата возврата:</span>
          <div class="flex-1 border-b border-dashed"></div>
          <b>11.08.2024</b>
        </div>

        <button
          :disabled="buttonDisabled"
          @click="createOrder"
          class="mt-4 transition bg-lime-500 w-full rounded-xl py-3 disabled:bg-slate-300 text-white hover:bg-lime-600 active:bg-lime-700 cursor-pointer"
        >
          Оформить
        </button>
      </div>
    </div>
  </div>
</template>
