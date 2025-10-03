<!-- eslint-disable vue/multi-word-component-names -->
<script setup>
import { inject } from 'vue'
defineProps({
  id: Number,
  imageUrl: String,
  title: String,
  price: Number,
  isFavorite: Boolean,
  isAdded: Boolean,
  onClickAdd: Function,
  onClickFavorite: Function,
})
// const onClickAdd = () => {
//   alert("Добавлен")
// }

</script>

<template>
  <div
    class="relative bg-white border-slate-100 rounded-3xl p-8 cursor-pointer transition-all shadow hover:-translate-y-2 hover:shadow-xl"
  >
    <div>
      <img
        alt="Like 2"
        :src="!isFavorite ? '/like-1.svg' : '/like-2.svg'"
        class="absolute top-8 left-8"
        @click="onClickFavorite"
      />

      <img :src="imageUrl" alt="Sneaker" />
      <p class="mt-2">{{ title }}</p>

      <div class="flex justify-between mt-5">
        <div class="flex flex-col">
          <span class="text-slate-400">Цена: </span>
          <b>{{ price }} руб.</b>
        </div>
        <img :src="!isAdded ? '/plus.svg' : '/checked.svg'" alt="Plus" @click="onClickAdd" />
      </div>
    </div>
  </div>
</template>
