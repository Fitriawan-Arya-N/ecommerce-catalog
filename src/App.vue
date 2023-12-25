<!-- src/App.vue -->

<template>
  <div id="app">
    <ProductDisplay />
  </div>
</template>

<style>
@import './assets/style/page.css';
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;600&display=swap');
</style>

<script>
import ProductDisplay from "@/components/ProductDisplay.vue";

export default {
    name: 'App',
    components: {
    ProductDisplay,
  },
};
</script>
