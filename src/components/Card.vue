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
  <div
    class="bg-white relative border border-slate-100 rounded-3xl p-8 transition cursor-pointer hover:-translate-y-2 hover:shadow-xl"
  >
    <img
      v-if="onClickFavorite"
      :src="!isFavorite ? '/like-1.svg' : '/like-2.svg'"
      alt="Like"
      class="absolute top-8 left-8"
      @click="onClickFavorite"
    />
    <img class="object-scale-down h-48" :src="imageUrl" alt="Sneakers" />
    <p class="mt-2 ">{{ title }}</p>
    

    <div class="flex justify-between mt-5">
      <div class="flex flex-col absolute bottom-1.5 ">
        <span class="text-slate-400">Цена:</span>
        <b>{{ price }} usd</b>
      </div>
      <img class="absolute bottom-4 right-4"
        v-if="onClickAdd"
        @click="onClickAdd"
        :src="!isAdded ? '/plus.svg' : '/checked.svg'"
        alt="Plus"
      />
    </div>
  </div>
</template>
