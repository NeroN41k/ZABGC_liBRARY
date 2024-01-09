<script setup>
import Card from './Card.vue'
const onClickAdd = () => {
  alert('Добавить!')
}
</script>

<template>
  <div class="grid grid-cols-4 gap-5">
    <Card
      author="Джером Д. Сэлинджер"
      title="Над пропастью во ржи"
      imageUrl="/book-cart.svg"
      :count="5"
      :isAdded="true"
      :isFavorite="false"
      :onClickAdd="onClickAdd"
    />
    <Card
      author="Джейн Остен"
      title="Гордость и предубеждение"
      imageUrl="/book-cart.svg"
      :count="6"
      :isAdded="false"
      :isFavorite="true"
    />
    <Card
      author="Фрэнсис Скотт Фицджеральд"
      title="Великий Гэтсби"
      imageUrl="/book-cart.svg"
      :count="3"
      :isAdded="false"
      :isFavorite="false"
    />
    <Card
      author="Михаил Булгаков"
      title="Мастер и Маргарита"
      imageUrl="/book-cart.svg"
      :count="2"
      :isAdded="true"
      :isFavorite="false"
    />
    <Card
      author="Антуан де Сент-Экзюпери"
      title="Маленький принц"
      imageUrl="/book-cart.svg"
      :count="4"
      :isAdded="false"
      :isFavorite="true"
    />
    <Card
      author="Джон Фаулз"
      title="Коллекционер"
      imageUrl="/book-cart.svg"
      :count="7"
      :isAdded="true"
      :isFavorite="false"
    />
  </div>
</template>
