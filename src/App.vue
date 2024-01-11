<script setup>
import { onMounted } from 'vue'
import axios from 'axios'

import Header from './components/Header.vue'
import CardList from './components/CardList.vue'
import Drawer from './components/Drawer.vue'

onMounted(() => {
  axios.get('').then((resp) => console.log(resp.data))
})
</script>

<template>
  <!--<Drawer /> -->
  <div class="bg-white w-3/5 m-auto rounded-xl shadow-xl shadow-grey-200 mt-10 mb-5">
    <Header />

    <div class="p-10">
      <div class="flex justify-between items-center">
        <h2 class="text-3xl font-bold mb-8">Все книги</h2>

        <div class="flex gap-4">
          <!--<select class="py-2 px-3 border rounded-md outline-none">
            <option>По названию</option>
          </select> хуй знает зачем здесь сортировка💩 -->

          <div class="relative">
            <img class="absolute left-3 top-3" src="/search.svg" alt="Search" />
            <input
              class="border rounded-md py-2 pl-11 pr-4 outline-none focus:border-gray-400"
              type="text"
              placeholder="Поиск..."
            />
          </div>
        </div>
      </div>

      <div class="mt-10">
        <CardList />
      </div>
    </div>
  </div>
</template>
