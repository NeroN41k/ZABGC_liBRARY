<script setup>
import Header from './components/Header.vue'
import CardList from './components/CardList.vue'
import Drawer from './components/Drawer.vue'
</script>

<template>
  <!--<Drawer /> -->
  <div class="bg-white w-3/5 m-auto rounded-xl shadow-xl shadow-grey-200 mt-10 mb-5">
    <Header />

    <div class="p-10">
      <h2 class="text-3xl font-bold mb-8">Все книги</h2>

      <CardList />
    </div>
  </div>
</template>
