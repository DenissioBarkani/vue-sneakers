<!-- eslint-disable vue/multi-word-component-names -->
<script setup>
import { computed } from 'vue'
import CartItemList from './CartItemList.vue'
import DrawerHead from './DrawerHead.vue'
import InfoBlock from './InfoBlock.vue'
defineProps({
  totalPrice: Number,
  vatPrice: Number,
  disabledButton: Boolean,
})

const emit = defineEmits(['createOrder'])
</script>

<template>
  <div class="fixed top-0 left-0 h-full w-full bg-black bg-opacity-70 z-10"></div>
  <div class="bg-white w-96 h-full fixed right-0 top-0 z-20 p-8 flex flex-col">
    <DrawerHead />

    <InfoBlock
      v-if="!totalPrice"
      title="Корзина пустая"
      description="Добавьте хотя бы одну пару кроссовок."
      image-url="/package-icon.png"
    />

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
      :disabled="disabledButton"
      class="transition bg-lime-500 w-full disabled:cursor-not-allowed disabled:bg-slate-300 rounded-xl py-3 text-white hover:bg-lime-600 active:bg-lime-700 cursor-pointer"
      @click="() => emit('createOrder')"
    >
      Оформить заказ
    </button>
  </div>
</template>
