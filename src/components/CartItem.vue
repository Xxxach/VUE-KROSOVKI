<script setup>
const emit = defineEmits(['onClickRemove']);

defineProps({
  id: Number,
  title: String,
  imageUrl: String,
  price: Number,
});
</script>

<template>
  <div class="flex border border-slate-200 p-4 rounded-xl gap-4">
    <img class="w-16 h-16" :src="'/VUE-KROSOVKI' + imageUrl" alt="sneak" />
    <div class="flex flex-col flex-1">
      <p>{{ title }}</p>
      <div class="flex justify-between mt-2">
        <b class="flex-1">{{ price }}</b>
        <img
          @click="emit('onClickRemove')"
          class="opacity-40 cursor-pointer transition duration-300 ease-flyme hover:opacity-100"
          :src="'/VUE-KROSOVKI/close.svg'"
          alt="Close"
        />
      </div>
    </div>
  </div>
</template>
