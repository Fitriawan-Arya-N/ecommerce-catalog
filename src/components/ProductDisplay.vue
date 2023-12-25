<!-- src/components/ProductDisplay.vue -->

<template>
  <div class="card-container">
    <div v-if="loading">Loading...</div>
    <div v-else>
      <div v-for="product in displayedProducts" :key="product.id" class="product-card">
        <img :src="product.image" :alt="product.title" />
        <div class="product-info">
            <h3>{{ product.title }}</h3>
            <p>{{ product.category }}</p>
            <p>{{ product.description }}</p>
            <p>Harga: ${{ product.price }}</p>
        </div>
      </div>
      <button @click="nextProduct">Next Product</button>
    </div>
  </div>
</template>





<script>
export default {
  data() {
    return {
      displayedProducts: [],
      loading: true,
      currentIndex: 1,
    };
  },
  mounted() {
    this.fetchProducts();
  },
  methods: {
    fetchProducts() {
      fetch(`https://fakestoreapi.com/products/${this.currentIndex}`)
        .then(response => response.json())
        .then(data => {
          if (data.category === "men's clothing" || data.category === "women's clothing") {
            this.displayedProducts = [data];
          } else {
            this.displayedProducts = [];
          }
          this.loading = false;
        })
        .catch(error => {
          console.error('Error fetching data:', error);
          this.loading = false;
        });
    },
    nextProduct() {
      this.currentIndex = (this.currentIndex % 20) + 1;
      this.fetchProducts();
    },
  },
};
</script>

