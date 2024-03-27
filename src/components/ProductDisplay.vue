<template>
  <!-- 1. main -->
  <main class="bg" :style="{ background: currentProduct.category === 'men\'s clothing' ? 'linear-gradient(to top, white 0%, white 35%, #D6E6FF 35%, #D6E6FF 100%)' : (currentProduct.category === 'women\'s clothing' ? 'linear-gradient(to top, white 0%, white 35%, #fde2ff 35%, #fde2ff 100%)' : 'linear-gradient(to top, white 0%, white 35%, #D8D7D7 35%, #D8D7D7 100%)') }" >
    <!-- 2. content-->
    <div class="card" >
      <!-- Loading Check -->
      <!-- women's & men's/other check -->
        <div v-if="currentProduct.category === 'men\'s clothing' || currentProduct.category === 'women\'s clothing'" class="product-container">
            <!-- <div v-if="currentProduct" class="product-container" >  -->
              <!-- 3. content image -->
              <div class="product-image">
                <img :src="currentProduct.image" alt="Product Image" class="featured-image">
              </div>
              <!-- 4. content description -->
              <div class="product-description">
                <div class="description-container">
                  <h1 class="title" :style="{ color: currentProduct.category === 'men\'s clothing' ? '#002772' : (currentProduct.category === 'women\'s clothing' ? '#720060' : 'black') }">{{ currentProduct.title }}</h1>
                <div class="rate-container">
                  <span class="category">{{ currentProduct.category }}</span>
                  <div class="rating-rate">
                    <div class="rate">{{ currentProduct.rating.rate }}/5</div>
                    <div v-if="currentProduct.category === 'men\'s clothing'" class="star-container">
                      <div v-for="star in 5" :key="star" :class="{ 'star-men': true, 'active-men': star <= currentProduct.rating.rate }"></div>
                    </div>
                    <div v-else class="star-container">
                      <div v-for="star in 5" :key="star" :class="{ 'star-women': true, 'active-women': star <= currentProduct.rating.rate }"></div>
                    </div>
                    <!-- <div v-for="star in 5" :key="star" :class="starClass(star)"></div> -->
                </div>
                </div>
                <hr>
                  <p class="description">{{ currentProduct.description }}</p>
                </div>
                  
                <hr>
                  <h1 class="price dollar" :style="{ color: currentProduct.category === 'men\'s clothing' ? '#002772' : (currentProduct.category === 'women\'s clothing' ? '#720060' : 'black') }">{{ currentProduct.price }}</h1>
                  <div class="button-container">
                    <button class="button-left" :style="{ backgroundColor: currentProduct.category === 'men\'s clothing' ? '#002772' : (currentProduct.category === 'women\'s clothing' ? '#720060' : 'black') }">Buy Now</button>
                    <button class="button-right" @click="getNextProduct" :disabled="loading" :style="{ color: currentProduct.category === 'men\'s clothing' ? '#002772' : (currentProduct.category === 'women\'s clothing' ? '#720060' : 'black'), outline: currentProduct.category === 'men\'s clothing' ? '0.2rem solid #002772' : (currentProduct.category === 'women\'s clothing' ? '0.2rem solid #720060' : '0.2rem solid black') }">Next Product</button>
                  </div>
                </div>
              <!-- </div> -->
        </div>
        <!-- other -->
        <div v-else class="product-container other">
            <p>This product is unavailable to show</p>
            <button class="button-unavailable" @click="getNextProduct" :disabled="loading">Next Product</button>
        </div>
    </div>
  </main>
</template>

<script>
import axios from 'axios';

export default {
  name: 'ProductDisplay',
  data() {
    return {
      products: [],
      currentProductIndex: 0,
      currentProduct: null,
    };
  },

  methods: {
    setProducts(data) {
      this.products = data;
      this.currentProduct = this.products[this.currentProductIndex];
    },
    getNextProduct() {
      if (this.currentProductIndex < this.products.length - 1) {
        this.currentProductIndex++;
      } else {
        this.currentProductIndex = 0;
      }
      this.currentProduct = this.products[this.currentProductIndex];
      this.loading = false;
    },
  },
  mounted() {
    axios
      .get("https://fakestoreapi.com/products")
      .then(response => this.setProducts(response.data))
      .catch(error => console.error(error));
  }
};
</script>