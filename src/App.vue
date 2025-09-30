<script setup>
import MyHello from './components/MyHello.vue'
</script>

<template>
  <div class="mb-40">
    <MyHello title="Кроссовки Пума" :price="9500" :isAdded="true"></MyHello>
  </div>
  <div class="mb-40">
    <MyHello title="Кроссовки Пума" :price="9500" :isAdded="true"></MyHello>
  </div>
</template>

<style scoped></style>
