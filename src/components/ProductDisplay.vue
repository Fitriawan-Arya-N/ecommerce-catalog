<!-- src/components/ProductDisplay.vue -->

<template>
  <div class="card-container">
    <div v-if="loading">Loading...</div>
    <div v-else>
      <div v-for="product in displayedProducts" :key="product.id" class="product-card">
        <div class="content">


          <div class="image">
            <img :src="product.image" :alt="product.title" />
          </div>


          <div class="text">
            <h2>{{ product.title }}</h2>
            <p>{{ product.category }}</p>
            <hr />
            <div class="ProdDescription">{{ product.description }}</div>
          </div>
        </div>


            <div class="text2">
                <hr />
                <h2 class="price">${{ product.price }}</h2>
                <div class="btn">
                    <button class="btn-buy">Buy now</button>
                    <button class="btn-next" @click="nextProduct">Next product</button>
                </div>
            </div>




      </div>
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

