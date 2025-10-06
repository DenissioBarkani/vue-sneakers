<script setup>
import { inject } from 'vue'
import Card from './Card.vue'

defineProps({
  items: Array,
  isFavorites: Boolean,
})

const emit = defineEmits(['addToFavorite', 'addToCart'])

</script>

<template>
  <div v-auto-animate class="grid grid-cols-4 gap-5">
    <Card
      v-for="item in items"
      :id="item.id"
      :key="item.id"
      :title="item.title"
      :image-url="item.imageUrl"
      :price="item.price"
      :is-added="item.isAdded"
      :is-favorite="item.isFavorite"
      :on-click-favorite="() => emit('addToFavorite', item)"
      :on-click-add="isFavorites ? null :() => emit('addToCart', item)"
    />
  </div>
</template>

<!-- :is-favorite="true"
      :onClickAdd="onClickAdd"
      onClickFavorite="" -->
