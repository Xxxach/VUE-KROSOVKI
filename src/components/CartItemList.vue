<script setup>
import { inject } from 'vue';
import CartItem from './CartItem.vue';
import { ref, onMounted } from 'vue';

const isReady = ref(false);

onMounted(() => {
  // Даем окну 0.6с, чтобы оно полностью выехало и успокоилось
  setTimeout(() => {
    isReady.value = true;
  }, 600);
});

const { cart, removeFromCart } = inject('cart');
</script>

<template>
  <div
    class="flex flex-col flex-1 gap-4 justify-between opacity-0 animate-fade-up"
  >
    <CartItem
      v-for="item in cart"
      :key="item.id"
      :title="item.title"
      :price="item.price"
      :image-url="item.imageUrl"
      @on-click-remove="() => removeFromCart(item)"
    />
  </div>
</template>
