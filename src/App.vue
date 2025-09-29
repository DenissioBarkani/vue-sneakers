<script setup>
import MyHello from './components/MyHello.vue'
</script>

<template>
  <MyHello title="Кроссовки Пума" :price="9500" :isAdded="true"> <button>123</button></MyHello>

  <MyHello />

  <MyHello />
</template>

<style scoped></style>
