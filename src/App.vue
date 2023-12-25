<!-- src/App.vue -->

<template>
  <div id="app">
    <ProductDisplay />
  </div>
</template>

<style>
@import './assets/style/page.css';
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
