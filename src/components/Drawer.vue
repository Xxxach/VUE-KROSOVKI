<script setup>
import axios from 'axios';
import { ref, watch, computed, inject } from 'vue';

import DrawerHead from './DrawerHead.vue';
import CartItemList from './CartItemList.vue';
import InfoBlock from './InfoBlock.vue';

const props = defineProps({
  totalPrice: Number,
  vatPrice: Number,
});

const { cart, closeDrawer } = inject('cart');

const isCreating = ref(false);
const orderId = ref(null);

const createOrder = async () => {
  try {
    isCreating.value = true;
    const { data } = await axios.post(
      `https://d871c578df71a2ff.mokky.dev/orders`,
      {
        items: cart.value,
        totalPrice: props.totalPrice.value,
      },
    );

    cart.value = [];

    orderId.value = data.id;
  } catch (err) {
    console.log(err);
  } finally {
    isCreating.value = false;
  }
};

const cartIsEmpty = computed(() => cart.value.length === 0);

const buttonDisabled = computed(() => isCreating.value || cartIsEmpty.value);
</script>

<template>
  <div class="fixed top-0 left-0 h-full w-full z-20">
    <div
      class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-70"
    ></div>
    <div class="bg-white w-96 h-full fixed top-0 right-0 z-20 p-8">
      <DrawerHead />

      <div v-if="!totalPrice || orderId" class="flex h-full items-center">
        <InfoBlock
          v-if="!totalPrice && !orderId"
          title="Корзина пуста"
          description="Добавьте товар"
          :imageUrl="'/VUE-KROSOVKI/package-icon.png'"
        />
        <InfoBlock
          v-if="orderId"
          title="Заказ оформлен"
          :description="`Ваш заказ #${orderId} обрабатывается на базе`"
          :imageUrl="'/VUE-KROSOVKI/order-success-icon.png'"
        />
      </div>

      <div v-else>
        <CartItemList />

        <div class="flex flex-col gap-4 mt-7">
          <div class="flex gap-2">
            <span>Итого:</span>
            <div class="flex-1 border-b border-dashed"></div>
            <b>{{ totalPrice }} ₽</b>
          </div>

          <div class="flex gap-2">
            <span>Налог:</span>
            <div class="flex-1 border-b border-dashed"></div>
            <b>{{ vatPrice }} ₽</b>
          </div>

          <button
            :disabled="buttonDisabled"
            @click="createOrder"
            class="mt-4 bg-red-800 w-full rounded-xl py-3 text-white cursor-pointer transition duration-500 ease-flyme disabled:bg-slate-300 disabled:scale-100 hover:bg-red-700 hover:scale-101 active:bg-red-900 active:scale-98"
          >
            Оформить заказ
          </button>
        </div>
      </div>
    </div>
  </div>
</template>
