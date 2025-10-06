<script setup>
import Header from './components/Header.vue'

import { computed, provide, ref, watch } from 'vue'
import axios from 'axios'
import Drawer from './components/Drawer.vue'


// Корзина
const cart = ref([])
const totalPrice = computed(() => cart.value.reduce((acc, item) => acc + item.price, 0))
const vatPrice = computed(() => Math.round((totalPrice.value * 5) / 100))
const isCreatingOrder = ref(false)

const cartIsEmpy = computed(() => cart.value.length === 0)
const cartButtonDisabled = computed(() => isCreatingOrder.value || cartIsEmpy.value)

const createOrder = async () => {
  try {
    isCreatingOrder.value = true
    const { data } = await axios.post('https://34530b615c1cfbfa.mokky.dev/orders', {
      items: cart.value,
      totalPrice: totalPrice.value,
    })
    cart.value = []

    return data
  } catch (err) {
    console.log(err)
  } finally {
    isCreatingOrder.value = false
  }
}

watch(
  cart,
  () => {
    localStorage.setItem('cart', JSON.stringify(cart.value))
  },
  { deep: true },
)
// Корзина конец

// watch(
//   cart,
//   () => {
//     items.value = items.value.map((item) => ({
//       ...item,
//       isAdded: cart.value.some((cartItem) => cartItem.id === item.id),
//     }))
//     localStorage.setItem('cart', JSON.stringify(cart.value))
//   },
//   { deep: true }
// )

// const addToCart = (item) => {
//   cart.value.push(item)
//   item.isAdded = true
// }
// const removeFromCart = (item) => {
//   cart.value.splice(cart.value.indexOf(item), 1)
//   item.isAdded = false
// }

const addToCart = (item) => {
  // не добавляем дубликаты
  if (!cart.value.some((ci) => ci.id === item.id)) {
    cart.value.push(item)
  }
  item.isAdded = true
}
const removeFromCart = (item) => {
  const idx = cart.value.findIndex((ci) => ci.id === item.id)
  if (idx !== -1) {
    cart.value.splice(idx, 1)
  }
  item.isAdded = false
}

const drawerOpen = ref(false)
const closeDrawer = () => {
  drawerOpen.value = false
}
const openDrawer = () => {
  drawerOpen.value = true
}
provide('cart', {
  cart,
  closeDrawer,
  openDrawer,
  addToCart,
  removeFromCart,
})
</script>

<template>
  <Drawer
    v-if="drawerOpen"
    :total-price="totalPrice"
    :vat-price="vatPrice"
    :disabled-button="cartButtonDisabled"
    @create-order="createOrder"
  />
  <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-10">
    <Header :total-price="totalPrice" @open-drawer="openDrawer" />

    <div class="p-10">
      <router-view></router-view>
      <Home />
    </div>
  </div>
</template>

<style scoped></style>
