<script setup>
import CartItemList from './CartItemList.vue'
import DrawerHead from './DrawerHead.vue'
</script>

<template>
  <div class="fixed top-0 left-0 h-full w-full bg-black bg-opacity-70 z-10"></div>
  <div class="bg-white w-96 h-full fixed right-0 top-0 z-20 p-8 flex flex-col" >
    <DrawerHead />

    <CartItemList />
    <div class="flex flex-col gap-3 my-7">
        <div class="flex gap-2">
            <span>Итого:</span>
            <div class="flex-1 border-b border-dashed"></div>
            <b>12900 Р</b>
        </div>
        <div class="flex gap-2">
            <span>Налог 5%:</span>
            <div class="flex-1 border-b border-dashed"></div>
            <b>900 Р</b>
        </div>
    </div>
    <button disabled="" class="transition bg-lime-500 w-full disabled:bg-slate-300 rounded-xl py-3 text-white hover:bg-lime-600 active:bg-lime-700 cursor-pointer">
        Оформить заказ
    </button>
  </div>
</template>
