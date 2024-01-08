<template>
  <header class="flex justify-between border-b border-slate-200 px-10 py-8">
    <div class="flex items-center gap-4">
      <img src="/zabgc.png" alt="Logo" class="w-12" />
      <div>
        <h2 class="font-bold text-xl">Библиотека ЗабГК</h2>
        <p class="text-slate-400">Все книги горного дела в одном месте</p>
      </div>
    </div>
    <ul class="flex items-center gap-6">
      <li class="flex items-center gap-3 text-gray-500 hover:text-black cursor-pointer">
        <img src="/book.svg" alt="Book" />
        <b>Выбрано 3</b>
      </li>
      <li class="flex items-center gap-3 text-gray-500 hover:text-black cursor-pointer">
        <img src="/heart.svg" alt="Favorite" />
        <span>Закладки</span>
      </li>
      <li class="flex items-center gap-3 text-gray-500 hover:text-black cursor-pointer">
        <img src="/profile.svg" alt="Favorite" />
        <span>Профиль</span>
      </li>
    </ul>
  </header>
</template>
