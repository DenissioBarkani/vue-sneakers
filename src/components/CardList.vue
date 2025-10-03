<script setup>
import { inject } from 'vue'
import Card from './Card.vue'

defineProps({
  items: Array,
})

const onClickAdd = () => {
  alert('Добавить!')
}
const emit = defineEmits(['addToFavorite'])

// const addToFavorite = inject('addToFavorite')
// const onClickFavorite = () => {
//   const obj = {
//     ...props,
//     parentId: props.id,
//   }
//   addToFavorite(obj)
// }
// const addToFavorite = () => {}
</script>

<template>
  <div class="grid grid-cols-4 gap-5">
    <Card
      v-for="item in items"
      :id="item.id"
      :key="item.id"
      :title="item.title"
      :image-url="item.imageUrl"
      :price="item.price"
      :is-added="true"
      :is-favorite="item.isFavorite"
      :on-click-favorite="() => emit('addToFavorite', item)"
    />
  </div>
</template>

<!-- :is-favorite="true"
      :onClickAdd="onClickAdd"
      onClickFavorite="" -->
