<!-- eslint-disable vue/multi-word-component-names -->
<script setup>
import { inject, onMounted, reactive, ref, watch } from 'vue'
import CardList from '../components/CardList.vue'
import debounce from 'lodash.debounce'
import axios from 'axios'
const {cart, addToCart, removeFromCart } = inject('cart')
const items = ref([])
const filters = reactive({
  sortBy: 'title',
  searchQuery: '',
})
const onChangeSelect = (event) => {
  filters.sortBy = event.target.value
}

const onChangeInput = debounce((event) => {
  filters.searchQuery = event.target.value
}, 300)

const addToFavorite = async (item) => {
  try {
    if (!item.isFavorite) {
      const obj = {
        item_id: item.id,
      }
      const { data } = await axios.post('https://34530b615c1cfbfa.mokky.dev/favorites', obj)
      item.isFavorite = true
      item.favoriteId = data.id
      console.log(data)
    } else {
      await axios.delete(`https://34530b615c1cfbfa.mokky.dev/favorites/${item.favoriteId}`)
      item.isFavorite = false
      item.favoriteId = null
    }
  } catch (err) {
    console.log(err)
  }
}
const onClickAddPlus = (item) => {
  if (!item.isAdded) {
    addToCart(item)
  } else {
    removeFromCart(item)
  }
}

const fetchFavorites = async () => {
  try {
    const { data } = await axios.get('https://34530b615c1cfbfa.mokky.dev/favorites')

    items.value = items.value.map((item) => {
      const favorite = data.find((favorite) => favorite.item_id === item.id)

      if (!favorite) {
        return item
      }
      return {
        ...item,
        isFavorite: true,
        favoriteId: favorite.id,
      }
    })

    console.log('Items after favorites:', items.value) // И этот
  } catch (e) {
    console.log(e)
  }
}
const fetchItems = async () => {
  try {
    const params = {
      sortBy: filters.sortBy,
    }
    if (filters.searchQuery) {
      params.title = `*${filters.searchQuery}*`
    }
    const { data } = await axios.get('https://34530b615c1cfbfa.mokky.dev/items', {
      params,
    })

    items.value = data.map((obj) => ({
      ...obj,
      isFavorite: false,
      favoriteId: null,
      isAdded: false,
    }))
    // console.log(items.value)
  } catch (e) {
    console.log(e)
  }
}

watch(
  cart,
  () => {
    items.value = items.value.map((item) => ({
      ...item,
      isAdded: false,
    }))

  })
watch(filters, fetchItems)
onMounted(async () => {
  const localCart = localStorage.getItem('cart')
  cart.value = localCart ? JSON.parse(localCart) : []

  await fetchItems()
  await fetchFavorites()


  items.value = items.value.map((item) => ({
    ...item,
    isAdded: cart.value.some((cartItem)=> cartItem.id=== item.id)
  }))
})




</script>

<template>
  <div class="flex justify-between items-center">
    <h2 class="text-3xl font-bold mb-8">Все кроссовки</h2>
    <div class="flex gap-4">
      <select class="py-2 px-3 border rounded-md outline-none" @change="onChangeSelect">
        <option class="" value="name">По названию</option>
        <option value="price">По цене (д)</option>
        <option value="-price">По цене (де)</option>
      </select>
      <div class="relative">
        <img class="absolute left-4 top-3" src="/search.svg" alt="" />
        <input
          class="border rounded-md py-2 pl-11 pr-4 outline-none focus:border-gray-400"
          type="text"
          placeholder="Поиск..."
          @input="onChangeInput"
        />
      </div>
    </div>
  </div>
  <div class="mt-10">
    <CardList :items="items" @add-to-favorite="addToFavorite" @add-to-cart="onClickAddPlus" />
  </div>
</template>
