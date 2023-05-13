<template>
  <div :class="['product-container', getClassByCategory()]">
    <div v-if="loading" class="loader">
      <div class="spinner"></div>
    </div>
    <div v-else>
      <div class="product-grid">
        <div class="product-item">
          <div class="product-content">
            <div class="product-image">
              <img v-if="isAvailableCategory" :src="currentProduct.image" alt="Product Image" />
              <p v-else>This product is unavailable to show</p>
            </div>
            <div class="product-info">
              <h2 v-if="isAvailableCategory" class="product-title">{{ currentProduct.title }}</h2>
              <p v-if="isAvailableCategory" class="category-info">{{ currentProduct.category }}</p>
              <p v-if="isAvailableCategory" class="product-description">{{ currentProduct.description }}</p>
              <p v-if="isAvailableCategory" class="product-price">{{ '$' + currentProduct.price }}</p>
              <div class="button-group" v-if="isAvailableCategory">
                <button class="buy-button">Buy</button>
                <button class="next-button" @click="nextProduct">Next</button>
              </div>
              <!-- Tambahkan kondisi v-else di bawah ini -->
              <div v-else>
                <button class="next-button" @click="nextProduct">Next</button>
              </div>
              <!-- Akhir tambahan -->
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
      products: [],
      currentProductIndex: 0,
      currentProduct: {},
      loading: true,
    };
  },
  mounted() {
    this.fetchProducts();
  },
  methods: {
    fetchProducts() {
      this.loading = true;
      fetch('https://fakestoreapi.com/products')
        .then((response) => response.json())
        .then((data) => {
          this.products = data;
          this.currentProduct = this.products[this.currentProductIndex];
          this.loading = false;
        })
        .catch((error) => {
          console.log(error);
          this.loading = false;
        });
    },
    nextProduct() {
      this.currentProductIndex++;
      if (this.currentProductIndex >= this.products.length) {
        this.currentProductIndex = 0;
      }
      this.currentProduct = this.products[this.currentProductIndex];
    },
    getClassByCategory() {
      const category = this.currentProduct.category;
      if (category === "men's clothing") {
        return ['page-men'];
      } else if (category === "women's clothing") {
        return ['page-women'];
      } else {
        return ['page-unavailable'];
      }
    },
  },
  computed: {
    isAvailableCategory() {
      const category = this.currentProduct.category;
      return category === "men's clothing" || category === "women's clothing";
    },
  },
};
</script>

<style src="@/assets/page.css" scoped></style>
