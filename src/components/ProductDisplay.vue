<template>
  <div>
    <div v-if="loading">
      <div class="view">
        <div class="loading-spinner"></div>
      </div>
    </div>

    <div v-else>
      <div v-if="displayedProducts.length > 0 && !category.unavailable" :class="getBackgroundClass()">
        <div class="container">
          <div class="product-card">
            <div class="image">
              <img :src="displayedProducts[0].image" :alt="displayedProducts[0].title" />
            </div>
            <div class="content">
              <div class="text">
                <h2 :style="{ color: h2Color }">{{ displayedProducts[0].title }}</h2>
                <div class="product-info">
                  
                  <p class="category">
                    {{ displayedProducts[0].category }}
                  </p>

                  <div v-if="isWomenCategory || isMenCategory" class="rating-container">
                    <div class="rating">
                      {{ displayedProducts[0].rating.rate }}/5
                    </div>

                    <div class="circle-container">
                      <div v-for="i in 5" :key="i" class="circle" :class="{
                          'filled': i <= Math.floor(displayedProducts[0].rating.rate),
                          'circle-woman-filled': i <= Math.floor(displayedProducts[0].rating.rate) && isWomenCategory,
                          'circle-man-filled': i <= Math.floor(displayedProducts[0].rating.rate) && isMenCategory
                        }"></div>
                    </div>
                  </div>

                </div>
                <hr class="garisatas" />
                <div class="ProdDescription">{{ displayedProducts[0].description }}</div>
              </div>
              <div class="text2">
                <hr class="garisbawah" />
                <h2 class="price">${{ displayedProducts[0].price }}</h2>
                <div class="btn">
                  <button class="btn-buy" :class="{ 'btn-men': isMenCategory, 'btn-women': isWomenCategory }">
                    Buy now
                  </button>
                  <button class="btn-next" @click="nextProduct"
                    :class="{ 'btn-men': isMenCategory, 'btn-women': isWomenCategory }">
                    Next product
                  </button>
                </div>
              </div>
            </div>

          </div>
        </div>
      </div>

      <div v-else class="unavailableView">
        <div class="BackgroundUnavailable"></div>
        <div class="unavailableCard">
          This product is unavailable to show
          <button type="button" @click="nextProduct" class="customNextButton">Next Product</button>
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
        maxIndex: 20,
        category: '',
      };
    },
    computed: {
      isMenCategory() {
        return (
          this.displayedProducts.length > 0 &&
          this.displayedProducts[0].category === "men's clothing"
        );
      },
      isWomenCategory() {
        return (
          this.displayedProducts.length > 0 &&
          this.displayedProducts[0].category === "women's clothing"
        );
      },
      h2Color() {
        return this.isMenCategory ? '#002772' : (this.isWomenCategory ? '#720060' : '');
      },
    },
    mounted() {
      this.fetchProducts();
    },
    methods: {
      fetchProducts() {
        this.loading = true;
        fetch(`https://fakestoreapi.com/products/${this.currentIndex}`)
          .then((response) => response.json())
          .then((data) => {
            if (
              data.category === "men's clothing" ||
              data.category === "women's clothing"
            ) {
              this.displayedProducts = [data];
            } else {
              this.displayedProducts = [];
            }
            this.loading = false;
          })
          .catch((error) => {
            console.error('Error fetching data:', error);
            this.loading = false;
          });
      },
      nextProduct() {
        this.currentIndex = (this.currentIndex % this.maxIndex) + 1;
        this.fetchProducts();
      },
      getBackgroundClass() {
        const category =
          this.displayedProducts.length > 0
            ? this.displayedProducts[0].category
            : '';
        if (category === "men's clothing") {
          return "BackgroundMan";
        } else if (category === "women's clothing") {
          return "BackgroundWoman";
        } else {
          return "BackgroundUnavailable";
        }
      },
    },
  };
</script>