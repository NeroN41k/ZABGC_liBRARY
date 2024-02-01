<script setup>
defineProps ({
    title: String,
    description: String,
    imageUrl: String
});
</script>

<template>
    <img height="70" width="70" :src="imageUrl" alt="InfoImage"/>
    <h2 class="mt-4 text-2xl font-medium">{{ title }}</h2>
    <p>{{ description }}</p>
</template>