<template>
  <div class="background-color">
    <div v-if="loading" class="loading"></div>
    <div v-else>
      <div v-if="product && (product.category === 'men\'s clothing' || product.category === 'women\'s clothing')">
        <div
          :class="{ 'men-background': product.category === 'men\'s clothing', 'women-background': product.category === 'women\'s clothing' }">
        </div>
        <div
          :class="{ 'men-section': product.category === 'men\'s clothing', 'women-section': product.category === 'women\'s clothing' }">
          <div class="product-card">
            <img :src="product.image" :alt="product.title" class="product-image">
            <div class="product-info">
              <h1>{{ product.title }}</h1>
              <div class="category-rating">
                <p class="category">{{ product.category }}</p>
                <div class="rating">
                  <span class="rating-value">{{ product.rating.rate.toFixed(1) }}/5</span>
                  <div class="rating-icons">
                    <div v-for="star in [1, 2, 3, 4, 5]" :key="star" class="rating-circle" :class="{ 'filled': star <= Math.round(product.rating.rate), 'women': product.category === 'women\'s clothing' }"></div>
                  </div>
                </div>
              </div>
              <p>{{ product.description }}</p>
              <div class="price-line">
                <p class="price"><span class="dollar-icon">$</span>{{ product.price }}</p>
              </div>
              <button class="buy-button">Buy Now</button>
              <button class="next-button" @click="nextProduct">Next Product</button>
            </div>
          </div>
        </div>
      </div>
      <div v-else>
        <div class="unavailable-background"></div>
        <div class="unavailable-section">
          <div class="product-card">
            <img src="../assets/Image/sad-face.png" alt="Unavailable Image" class="unavailable-image">
            <div class="overlay">
              <p>This product is unavailable to show.</p>
              <button class="next-button" @click="nextProduct">Next Product</button>
            </div>
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
      loading: false,
      product: null,
      index: 1
    };
  },
  methods: {
    getProduct(index) {
      this.loading = true;
      fetch(`https://fakestoreapi.com/products/${index}`)
        .then(response => response.json())
        .then(data => {
          this.loading = false;
          if (data.category === 'men\'s clothing' || data.category === 'women\'s clothing') {
            this.product = data;
          } else {
            this.product = null;
          }
        })
        .catch(error => {
          this.loading = false;
          console.error('Error:', error);
        });
    },
    nextProduct() {
      this.index = this.index < 20 ? this.index + 1 : 1;
      this.getProduct(this.index);
    },
  },
  mounted() {
    this.getProduct(this.index);
  }
};
</script>

<style scoped>
@import url('../assets/styles/style.css');
</style>
