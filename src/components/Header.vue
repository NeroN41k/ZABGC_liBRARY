<script setup>
defineProps({
  totalCountCart: Number
})

const emit = defineEmits(['openDrawer'])
</script>

<template>
  <header class="flex justify-between border-b border-slate-200 px-10 py-8">
    <router-link to="/">
      <div class="flex items-center gap-4">
        <img src="/zabgc.png" alt="Logo" class="w-12" />
        <div>
          <h2 class="font-bold text-xl">Библиотека ЗабГК</h2>
          <p class="text-slate-400">Все книги горного дела в одном месте</p>
        </div>
      </div></router-link
    >

    <ul class="flex items-center gap-6">
      <li
        @click="() => emit('openDrawer')"
        class="flex items-center gap-3 text-gray-500 hover:text-black cursor-pointer"
      >
        <img src="/book.svg" alt="Book" />
        <b>Выбрано {{ totalCountCart }}</b>
      </li>
      <router-link to="/favorites">
        <li class="flex items-center gap-3 text-gray-500 hover:text-black cursor-pointer">
          <img src="/heart.svg" alt="Favorite" />
          <span>Закладки</span>
        </li></router-link
      >
      <router-link to="/profile">
        <li class="flex items-center gap-3 text-gray-500 hover:text-black cursor-pointer">
          <img src="/profile.svg" alt="Favorite" />
          <span>Профиль</span>
        </li></router-link
      >
    </ul>
  </header>
</template>
