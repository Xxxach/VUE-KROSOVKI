<script setup>
defineProps({
  id: Number,
  title: String,
  imageUrl: String,
  price: Number,
  isFavorite: Boolean,
  isAdded: Boolean,
  onClickAdd: Function,
  onClickFavorite: Function,
});
</script>

<template>
  <!-- <div
    class="relative bg-white border border-slate-100 rounded-xl p-8 cursor-pointer transition ease-flyme duration-500 hover:-translate-y-2 hover:shadow-xl hover:shadow-bord active:-translate-y-1 active:opacity-90 active:shadow-bord-act/80"
  > -->
  <div
    class="relative bg-white border border-slate-100 rounded-xl p-8 cursor-pointer transition ease-flyme2 duration-500 hover:-translate-y-2 hover:shadow-xl hover:shadow-bord"
  >
    <img
      v-if="onClickAdd"
      @click="onClickFavorite"
      class="absolute top-8 left-8"
      :src="!isFavorite ? '/like-1.svg' : '/like-2.svg'"
      alt="LIKE1"
    />

    <img :src="imageUrl" alt="sneak" />
    <p class="mt-2">{{ title }}</p>

    <div class="flex justify-between mt-5">
      <div class="flex flex-col">
        <span class="text-slate-400">Цена</span>
        <b>{{ price }}</b>
      </div>

      <img
        v-if="onClickAdd"
        @click="onClickAdd"
        :src="!isAdded ? '/plus.svg' : '/checked.svg'"
        alt="PLUS"
      />
    </div>
  </div>
</template>
