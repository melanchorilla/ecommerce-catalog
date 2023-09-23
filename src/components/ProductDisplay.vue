<template>
    <section v-bind:class="{ men: isMen, women: isWomen, notFound: isNotFound }" v-if="!isNotFound">
      <main v-if="!isLoading">
        <div class="product-image">
          <img
            v-bind:src="this.product.image"
            alt=""
            class="image"
          />
        </div>
        <div class="product-description">
          <div class="description-container">
            <h2 class="title" v-bind:class="{ men: isMen, women: isWomen, notFound: isNotFound }">{{ this.product.title }}</h2>
            <div class="category-rating">
              <p class="category">{{ this.product.category }}</p>
              <p class="rating">{{ this.product.rating.rate }}/5</p>
            </div>
            <div class="description">
              <p>
                {{ this.product.description }}
              </p>
            </div>
            <div class="price" v-bind:class="{ men: isMen, women: isWomen, notFound: isNotFound }">
              <p>${{ this.product.price }}</p>
            </div>
            <div class="cta">
              <div class="buy-now" v-bind:class="{ men: isMen, women: isWomen }">
                <button>Buy now</button>
              </div>
    
              <div class="next-product" v-bind:class="{ men: isMen, women: isWomen }">
                <button @click="onClickNext">Next Product</button>
              </div>
            </div>
          </div>
        </div>
      </main>
      <div v-else class="loader"></div>
    </section>
    <section v-else>
      <main v-bind:class="{ men: isMen, women: isWomen, notFound: isNotFound }">
          <p>This product is unavailable to show</p>
          <button class="cta-404" @click="onClickNext">
              Next product
          </button>
        </main>
    </section>
  </template>
  
  <script>
  export default {
    data() {
      return {
        product: {},
        id: 1,
        isLoading: true,
        isMen: true,
        isWomen: false,
        isNotFound: false
      };
    },
    methods: {
      fetchProducts() {
        fetch(`https://fakestoreapi.com/products/${this.id}`)
          .then((res) => res.json())
          .then((json) => {
            console.log(this.id)
            this.product = json
            if (this.product.category === "men's clothing") {
              this.isMen = true
              this.isWomen = false
              this.isNotFound = false
            } else if (this.product.category === "women's clothing") {
              this.isWomen = true
              this.isMen = false
              this.isNotFound = false
            } else {
              this.isNotFound = true
            }
          });
      },
      onClickNext() {
        if (this.id < 20) {
          this.isLoading = true
          this.id++
          this.fetchProducts()
          this.isLoading = false
  
        } else {
          this.isLoading = true
          this.id = 1
          this.fetchProducts()
          this.isLoading = false
        }
      }
    },
    mounted() {
      this.fetchProducts()
      this.isLoading = false
    },
  };
  </script>
  