<script setup>
import { onMounted, ref } from 'vue';
import axios from 'axios';

import CardList from '../components/CardList.vue';

const favorites = ref([]);

onMounted(async () => {
  try {
    const { data } = await axios.get(
      'https://d871c578df71a2ff.mokky.dev/favorites?_relations=items',
    );

    favorites.value = data.map((obj) => obj.item);
  } catch (err) {
    console.log(err);
  }
});
</script>

<template>
  <h2 class="text-3xl font-bold mb-8">Мои закладки</h2>

  <CardList
    class="transition-transform ease-flyme2 duration-500 active:translate-y-1"
    :items="favorites"
    is-favorites
  />
</template>
