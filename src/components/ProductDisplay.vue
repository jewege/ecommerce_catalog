<template>
  <div class="container">
    <div v-if="isLoading" class="card">
      <div class="product-container">
        <div class="loader-container">
          <div class="loader"></div>
        </div>
      </div>
    </div>
    <div
      v-else
      class="container"
      :class="
        !isProductAvailable
          ? 'bg-light-gray'
          : product.data.category === 'men\'s clothing'
          ? 'bg-light-blue'
          : 'bg-light-pink'
      "
    >
      <div class="overlay">
        <img src="../assets/bg-shape.svg" alt="background overlay" />
      </div>
      <div class="card">
        <div v-if="!isProductAvailable" class="product-unavailable">
          <div class="overlay">
            <img
              src="../assets/bg-sad.svg"
              alt="unavailable product"
              srcset=""
            />
          </div>
          <div class="product-details">
            <p>This product is unavailable to show</p>
            <div class="button-container">
              <button
                type="button"
                @click="getSingleProduct()"
                class="btn-next"
              >
                Next Product
              </button>
            </div>
          </div>
        </div>
        <div v-else class="product-container">
          <div class="product-image">
            <img :src="product.data.image" alt="" />
          </div>
          <div class="product-details">
            <div class="product-title">
              <h3
                :class="
                  product.data.category === 'men\'s clothing'
                    ? 'font-navy'
                    : 'font-pink-dark'
                "
                class="title"
              >
                {{ product.data.title }}
              </h3>
              <div class="product-sub-title">
                <span>{{ product.data.category }}</span>
                <div class="rating">
                  <span>{{ product.data.rating.rate }}/5</span>
                  <div class="rating">
                    <span
                      :class="
                        product.data.category === 'men\'s clothing'
                          ? 'bg-navy'
                          : 'bg-pink-dark'
                      "
                      class="circle full"
                    ></span>
                    <span
                      :class="
                        product.data.category === 'men\'s clothing'
                          ? 'bg-navy'
                          : 'bg-pink-dark'
                      "
                      class="circle full"
                    ></span>
                    <span
                      :class="
                        product.data.category === 'men\'s clothing'
                          ? 'bg-navy'
                          : 'bg-pink-dark'
                      "
                      class="circle full"
                    ></span>
                    <span
                      :class="
                        product.data.category === 'men\'s clothing'
                          ? 'bg-navy'
                          : 'bg-pink-dark'
                      "
                      class="circle full"
                    ></span>
                    <span
                      :class="
                        product.data.category === 'men\'s clothing'
                          ? 'border-navy'
                          : 'border-pink-dark'
                      "
                      class="circle full"
                    ></span>
                  </div>
                </div>
              </div>
              <div class="description">
                <p>{{ product.data.description }}</p>
              </div>
            </div>
            <div class="price-color">
              <span
                :class="
                  product.data.category === 'men\'s clothing'
                    ? 'font-navy'
                    : 'font-pink-dark'
                "
                class="price"
                >${{ product.data.price }}</span
              >
              <div class="button-container">
                <button
                  type="button"
                  :class="
                    product.data.category === 'men\'s clothing'
                      ? 'bg-navy'
                      : 'bg-pink-dark'
                  "
                  class="btn-buy"
                >
                  Buy Now
                </button>
                <button
                  type="button"
                  @click="getSingleProduct()"
                  :class="
                    product.data.category === 'men\'s clothing'
                      ? 'border-btn-navy font-navy'
                      : 'border-btn-pink-dark font-pink-dark'
                  "
                  class="btn-next"
                >
                  Next Product
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: 'ProductDisplay',
  data () {
    return {
      isLoading: false,
      index: 0,
      isProductAvailable: false,
      product: {}
    }
  },
  methods: {
    async callAPI () {
      const response = await fetch(
        `https://fakestoreapi.com/products/${this.index}`
      )
      const result = await response.json()
      console.log(result)
      return result
    },
    async getSingleProduct () {
      this.isLoading = true

      this.index == 20 ? (this.index = 1) : this.index++

      let data = await this.callAPI()
      if (
        data.category === "men's clothing" ||
        data.category === "women's clothing"
      ) {
        this.product = { data }
        this.isProductAvailable = true
      } else {
        this.isProductAvailable = false
      }

      this.isLoading = false
    }
  },
  mounted () {
    this.getSingleProduct()
  }
}
</script>
<style>
@import '../assets/style/page.css';
</style>
