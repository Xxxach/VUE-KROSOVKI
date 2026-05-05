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
    <!-- Иконка лайка -->
    <img
      v-if="onClickAdd"
      @click="onClickFavorite"
      class="absolute top-8 left-8"
      :src="
        !isFavorite ? '/VUE-KROSOVKI/like-1.svg' : '/VUE-KROSOVKI/like-2.svg'
      "
      alt="LIKE"
    />

    <!-- Сами кроссовки -->
    <img :src="'/VUE-KROSOVKI' + imageUrl" alt="sneak" />

    <p class="mt-2">{{ title }}</p>

    <div class="flex justify-between mt-5">
      <div class="flex flex-col">
        <span class="text-slate-400">Цена</span>
        <b>{{ price }}</b>
      </div>

      <!-- Иконка плюсика / галочки -->
      <img
        v-if="onClickAdd"
        @click="onClickAdd"
        :src="!isAdded ? '/VUE-KROSOVKI/plus.svg' : '/VUE-KROSOVKI/checked.svg'"
        alt="PLUS"
      />
    </div>
  </div>
</template>
