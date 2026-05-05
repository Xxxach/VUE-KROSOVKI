<script setup>
import axios from 'axios';
import { onMounted, ref, reactive, provide, watch, computed } from 'vue';

import { inject } from 'vue';
import CardList from '../components/CardList.vue';

const { cart, addToCart, removeFromCart } = inject('cart');

const items = ref([]);

const filters = reactive({
  sortBy: 'title',
  searchQuery: '',
});

const onClickAddPlus = (item) => {
  if (!item.isAdded) {
    addToCart(item);
  } else {
    removeFromCart(item);
  }
};

const onChangeSelect = (event) => {
  filters.sortBy = event.target.value;
};

const onChangeSearchInput = (event) => {
  filters.searchQuery = event.target.value;
};

const addToFavorite = async (item) => {
  try {
    if (!item.isFavorite) {
      const obj = {
        parentId: item.id,
        item,
      };

      const { data } = await axios.post(
        `https://d871c578df71a2ff.mokky.dev/favorites`,
        obj,
      );

      item.isFavorite = true;
      item.favoriteId = data.id;
    } else {
      item.isFavorite = false;
      await axios.delete(
        `https://d871c578df71a2ff.mokky.dev/favorites/${item.favoriteId}`,
      );
      item.favoriteId = null;
    }
  } catch (err) {
    console.log(err);
  }
};

const fetchFavorites = async () => {
  try {
    const { data: favorites } = await axios.get(
      `https://d871c578df71a2ff.mokky.dev/favorites`,
    );

    items.value = items.value.map((item) => {
      const favorite = favorites.find((fav) => fav.parentId === item.id);

      if (!favorite) {
        return item;
      }

      return {
        ...item,
        isFavorite: true,
        favoriteId: favorite.id,
      };
    });
  } catch (err) {
    console.log(err);
  }
};

const fetchItems = async () => {
  try {
    const params = {
      sortBy: filters.sortBy,
    };

    if (filters.searchQuery) {
      params.title = `*${filters.searchQuery}*`;
    }

    const { data } = await axios.get(
      `https://d871c578df71a2ff.mokky.dev/items`,
      {
        params,
      },
    );

    items.value = data.map((obj) => ({
      ...obj,
      isFavorite: false,
      favoriteId: null,
      isAdded: false,
    }));
  } catch (err) {
    console.log(err);
  }
};

onMounted(async () => {
  const localCart = localStorage.getItem('cart');
  cart.value = localCart ? JSON.parse(localCart) : [];

  await fetchItems();
  await fetchFavorites();

  items.value = items.value.map((item) => ({
    ...item,
    isAdded: cart.value.some((cartItem) => cartItem.id === item.id),
  }));
});

watch(filters, async () => {
  await fetchItems();
  await fetchFavorites();
});

watch(cart, () => {
  items.value = items.value.map((item) => ({
    ...item,
    isAdded: false,
  }));
});
</script>

<template>
  <div class="flex justify-between items-center">
    <h2 class="text-3xl font-bold mb-8">Все россовки</h2>

    <div class="flex gap-4">
      <select
        @change="onChangeSelect"
        class="py-2 px-3 border border-gray-200 rounded-md outline-none"
        name=""
        id=""
      >
        <option value="name">По названию</option>
        <option value="price">Сначало дешевле</option>
        <option value="-price">Сначала дороже</option>
      </select>

      <div class="relative">
        <img class="absolute top-3 left-4" src="/search.svg" />
        <input
          @input="onChangeSearchInput"
          class="border border-gray-200 rounded-md py-2 pl-11 pr4 outline-none focus:border-gray-400"
          placeholder="Поиск..."
          type="text"
        />
      </div>
    </div>
  </div>

  <CardList
    :items="items"
    @add-to-favorite="addToFavorite"
    @add-to-cart="onClickAddPlus"
  />
</template>
