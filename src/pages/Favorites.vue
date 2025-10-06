<!-- eslint-disable vue/multi-word-component-names -->
<script setup>
import CardList from '../components/CardList.vue'
import axios from 'axios'
import { onMounted, ref } from 'vue'

const favorites = ref([])
onMounted(async () => {
  try {
    const { data } = await axios.get(
      'https://34530b615c1cfbfa.mokky.dev/favorites?_relations=items',
    )

    console.log(data)
    favorites.value = data.map((obj) => obj.item)
    console.log(favorites)
  } catch (err) {
    console.log(err)
  }
  // await fetchFavorites()
})
</script>
<template>
  <h2 class="text-3xl font-bold mb-8">Мои закалдки</h2>
  <div class="mt-10">
    <CardList :items="favorites" is-favorites />
  </div>
</template>
