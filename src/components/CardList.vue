<script setup>
import Card from './Card.vue';

defineProps({
  items: Array,
  isFavorites: Boolean,
});

const emit = defineEmits(['addToFavorite', 'addToCart']);
</script>

<template>
  <div class="grid grid-cols-4 gap-6">
    <div v-if="items.length === 0">Данных нет, массив пустой...</div>
    <div v-else>Загружено товаров: {{ items.length }}</div>

    <Card
      v-for="item in items"
      :key="item?.id"
      :id="item?.id"
      :title="item?.title"
      :image-url="
        item?.imageUrl
          ? (import.meta.env.BASE_URL + item.imageUrl).replace('//', '/')
          : ''
      "
      :price="item?.price"
      :is-favorite="item?.isFavorite"
      :onClickFavorite="isFavorites ? null : () => emit('addToFavorite', item)"
      :onClickAdd="isFavorites ? null : () => emit('addToCart', item)"
      :isAdded="item?.isAdded"
    />
  </div>
</template>
