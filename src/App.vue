<script setup lang="ts">
import { ref } from 'vue';

let clicks = ref(0)
let otch = ref(true)

let fun = (i: number) => {
  clicks.value += i
  otch.value = !otch.value
}
</script>

<template>
  <p>Накликали: {{ clicks }}</p>
  <p v-if="otch">Отличник</p>
  <p v-else>Не отличник</p>
  <button @click="fun(i)" v-for="i in 100">Привет {{ i  }}</button>
  <p>
    Visit <a href="https://vuejs.org/" target="_blank" rel="noopener">vuejs.org</a> to read the
    documentation
  </p>
</template>

<style scoped></style>
