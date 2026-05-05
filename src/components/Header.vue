<script setup>
defineProps({
  totalPrice: Number,
});

const emit = defineEmits(['openDrawer']);
</script>

<template>
  <header class="flex justify-between border-b border-slate-100 px-10 py-8">
    <router-link to="/">
      <div class="felx items-center gap-4">
        <img class="w-10" src="/logo.png" alt="logo" />
        <div>
          <h2 class="text-xl font-bold uppercase">Магазин Кросовок</h2>
          <p class="text-slate-500">Vue.js Diplom</p>
        </div>
      </div>
    </router-link>

    <ul class="flex items-center gap-8">
      <li
        @click="() => emit('openDrawer')"
        class="flex p-3 gap-3 items-center bg-white border border-slate-100 text-gray-500 rounded-xl cursor-pointer transition ease-flyme2 duration-500 hover:-translate-y-1 hover:shadow-lg hover:shadow-bord3 hover:text-bord2 active:-translate-y-0.5 active:shadow-bord-act"
      >
        <img src="/cart.svg" alt="CART" />
        <b>{{ totalPrice }}</b>
      </li>
      <router-link to="/favorites">
        <li
          class="flex p-3 gap-3 items-center bg-white border border-slate-100 text-gray-500 rounded-xl cursor-pointer transition ease-flyme2 duration-500 hover:-translate-y-1 hover:shadow-lg hover:shadow-bord3 hover:text-bord2 active:-translate-y-0.5 active:shadow-bord-act"
        >
          <img src="/heart.svg" alt="HEART" />
          <b>Закладки</b>
        </li>
      </router-link>
      <li
        class="flex p-3 gap-3 items-center bg-white border border-slate-100 text-gray-500 rounded-xl cursor-pointer transition ease-flyme2 duration-500 hover:-translate-y-1 hover:shadow-lg hover:shadow-bord3 hover:text-bord2 active:-translate-y-0.5 active:shadow-bord-act"
      >
        <img src="/profile.svg" alt="PROFILE" />
        <b>Профиле</b>
      </li>
    </ul>
  </header>
</template>
