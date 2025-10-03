<!-- eslint-disable vue/multi-word-component-names -->

<!-- опшенс апи -->
<!-- <script>
export default {
    data() {
        return {
            count: 0
        }
    },
    methods: {
        increment() {
            this.count++
        },
        decriment() {
            this.count--
        }
    }
}
</script> -->
<!-- <template>
  <p>{{ count }}</p>
  <button @click="increment">+</button>
</template>
 -->

 <!-- копозишон апи -->
<!-- <script setup>
import { ref } from 'vue'

const count = ref(0)
function increment() {
    count.value++
  console.log(count)
}
</script>

<template>
  <p>{{ count }}</p>
  <button @click="increment">+</button>
</template> -->



<!-- <script setup>
import { ref } from 'vue'

const count = ref(0)
function increment() {
    count.value++
  console.log(count)
}

// function updateCount(event) {
//     count.value = Number(event.target.value)
// }
</script>

<template>
  <p>{{ count }}</p>
  <button @click="increment">+</button>
  <input v-model="count"/>
</template> -->

<!--
<script setup>
defineProps({
  title: String,
  price: Number,
  isAdded: Boolean
})


</script>

<template>
  <div class="product">
    <h2>Товар: {{ title }}</h2>
    <h3>Цена: {{ price }}</h3>
    <h3>Добавлен: {{ isAdded }}</h3>
    <slot></slot>
  </div>
</template>

<style scoped>
.product {
  border: 1px solid blue;
  padding: 10px;
  margin-bottom: 10px;
}
</style> -->
