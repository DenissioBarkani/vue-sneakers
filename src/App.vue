<script setup>
import Header from './components/Header.vue'
import CardList from './components/CardList.vue'
import Drawer from './components/Drawer.vue'
import { onMounted, reactive, ref, watch } from 'vue'
import axios from 'axios'
// const items = [
//   {
//     id: 1,
//     title: 'Мужские Кроссовки Nike Blazer Mid Suede',
//     price: 12999,
//     imageUrl: '/sneakers/sneakers-1.jpg',
//   },
//   {
//     id: 2,
//     title: 'Мужские Кроссовки Nike Air Max 270',
//     price: 15600,
//     imageUrl: '/sneakers/sneakers-2.jpg',
//   },
//   {
//     id: 3,
//     title: 'Мужские Кроссовки Nike Blazer Mid Suede',
//     price: 8499,
//     imageUrl: '/sneakers/sneakers-3.jpg',
//   },
//   {
//     id: 4,
//     title: 'Кроссовки Puma X Aka Boku Future Rider',
//     price: 7800,
//     imageUrl: '/sneakers/sneakers-4.jpg',
//   },
//   {
//     id: 5,
//     title: 'Кроссовки Future Rider',
//     price: 9550,
//     imageUrl: '/sneakers/sneakers-5.jpg',
//   },
//   {
//     id: 6,
//     title: 'Кроссовки Black Edition',
//     price: 16999,
//     imageUrl: '/sneakers/sneakers-6.jpg',
//   },
//   {
//     id: 7,
//     title: 'Кроссовки Orange Boomb Edition',
//     price: 7499,
//     imageUrl: '/sneakers/sneakers-7.jpg',
//   },
//   {
//     id: 8,
//     title: 'Кроссовки Nike Air Max 270',
//     price: 15600,
//     imageUrl: '/sneakers/sneakers-8.jpg',
//   },
//   {
//     id: 9,
//     title: 'Кроссовки Nike Air Force 1',
//     price: 5900,
//     imageUrl: '/sneakers/sneakers-9.jpg',
//   },
//   {
//     id: 10,
//     title: 'Кроссовки Adidas Ultraboost',
//     price: 11500,
//     imageUrl: '/sneakers/sneakers-10.jpg',
//   },
//   {
//     id: 11,
//     title: 'Кроссовки Puma Clyde All-Pro',
//     price: 7600,
//     imageUrl: '/sneakers/sneakers-11.jpg',
//   },
//   {
//     id: 12,
//     title: 'Кроссовки Converse Chuck Taylor All-Star',
//     price: 13000,
//     imageUrl: '/sneakers/sneakers-12.jpg',
//   },
//   {
//     name: 'Root',
//     email: 'admin@email.com',
//     id: 13,
//   },
// ]
const items = ref([])

const filters = reactive({
  sortBy: 'title',
  searchQuery: '',
})
const fetchItems = async () => {
  try {
    const params = {
      sortBy: filters.sortBy,
      // searchQuery: filters.searchQuery,
    }
    if (filters.searchQuery) {
      params.title = `*${filters.searchQuery}*`;
    }
    const { data } = await axios.get('https://34530b615c1cfbfa.mokky.dev/items', {
      params
    })
    console.log(data)
    items.value = data
  } catch (e) {
    console.log(e)
  }
}

onMounted(fetchItems)
watch(filters, fetchItems)

// watch(filters, async () => {
//   try {
//     const { data } = await axios.get(
//       'https://34530b615c1cfbfa.mokky.dev/items?sortBy=' + filters.sortBy,
//     )
//     console.log(data)
//     items.value = data
//   } catch (e) {}
// })
const onChangeSelect = (event) => {
  filters.sortBy = event.target.value
}

const onChangeInput = (event) => {
  filters.searchQuery = event.target.value
}
</script>

<template>
  <!-- <Drawer /> -->
  <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-10">
    <Header />
    <div class="p-10">
      <div class="flex justify-between items-center">
        <h2 class="text-3xl font-bold mb-8">Все кроссовки</h2>
        <div class="flex gap-4">
          <select @change="onChangeSelect" class="py-2 px-3 border rounded-md outline-none">
            <option class="" value="name">По названию</option>
            <option value="price">По цене (д)</option>
            <option value="-price">По цене (де)</option>
          </select>
          <div class="relative">
            <img class="absolute left-4 top-3" src="/search.svg" alt="" />
            <input @input="onChangeInput"
              class="border rounded-md py-2 pl-11 pr-4 outline-none focus:border-gray-400"
              type="text"
              placeholder="Поиск..."
            />
          </div>
        </div>
      </div>
      <div class="mt-10">
        <CardList :items="items" />
      </div>
    </div>
  </div>
</template>

<style scoped></style>
