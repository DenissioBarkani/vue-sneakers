<!-- eslint-disable vue/multi-word-component-names -->
<script setup>
import { computed, inject, provide, ref } from 'vue'
import CartItemList from './CartItemList.vue'
import DrawerHead from './DrawerHead.vue'
import InfoBlock from './InfoBlock.vue'
import axios from 'axios'
const props = defineProps({
  totalPrice: Number,
  vatPrice: Number,
})
const { cart, closeDrawer } = inject('cart')
const isCreatingOrder = ref(false)
const cartIsEmpy = computed(() => cart.value.length === 0)
const buttonDisabled = computed(() => isCreatingOrder.value || cartIsEmpy.value)
const orderId = ref(null)
const createOrder = async () => {
  try {
    isCreatingOrder.value = true
    const { data } = await axios.post('https://34530b615c1cfbfa.mokky.dev/orders', {
      items: cart.value,
      totalPrice: props.totalPrice.value,
    })
    cart.value = []
    orderId.value = data.id
    return data
  } catch (err) {
    console.log(err)
  } finally {
    isCreatingOrder.value = false
  }
}

// const emit = defineEmits(['createOrder'])
</script>

<template>
  <div class="fixed top-0 left-0 h-full w-full bg-black bg-opacity-70 z-10"></div>
  <div class="bg-white w-96 h-full fixed right-0 top-0 z-20 p-8 flex flex-col">
    <DrawerHead />
    <div class="my-auto">
      <InfoBlock
        v-if="!totalPrice && !orderId"
        title="Корзина пустая"
        description="Добавьте хотя бы одну пару кроссовок."
        image-url="/package-icon.png"
      />
      <InfoBlock
        v-if="orderId"
        title="Заказ оформлен"
        :description="`Ваш заказ #${orderId} скоро будет передан курьерской доставке`"
        image-url="/order-success-icon.png"
      />
    </div>

    <CartItemList v-if="totalPrice" />
    <div v-if="totalPrice" class="flex flex-col gap-3 my-7">
      <div class="flex gap-2">
        <span>Итого:</span>
        <div class="flex-1 border-b border-dashed"></div>
        <b>{{ totalPrice }} Р</b>
      </div>
      <div class="flex gap-2">
        <span>Налог 5%:</span>
        <div class="flex-1 border-b border-dashed"></div>
        <b>{{ vatPrice }} Р</b>
      </div>
    </div>
    <button
      v-if="totalPrice"
      :disabled="buttonDisabled"
      class="transition bg-lime-500 w-full disabled:cursor-not-allowed disabled:bg-slate-300 rounded-xl py-3 text-white hover:bg-lime-600 active:bg-lime-700 cursor-pointer"
      @click="createOrder"
    >
      Оформить заказ
    </button>
  </div>
</template>
