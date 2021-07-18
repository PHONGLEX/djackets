<template>
  <div class="home">
    <div class="pricing-header px-3 py-3 pt-md-5 pb-md-4 mx-auto text-center">
      <h1 class="display-4">Welcome to Djacket</h1>
      <p class="lead">The best djacket store online.</p>
    </div>

    <div class="row">
      <div class="album py-5">
            <div class="row row-cols-1 row-cols-sm-2 row-cols-md-5 g-3">
                <ProductBox
                  v-for="product in latestProducts"
                  v-bind:key="product.id"
                  v-bind:product="product"
                 />
            </div>
        </div>
    </div>
  </div>
</template>

<style scoped>
  .card-img-top {
    width: 200px;
    height: 300px;
  }
</style>

<script>
  import axios from "axios"

  import ProductBox from '@/components/ProductBox.vue'

  export default {
    name: 'Home',
    data() {
      return {
        latestProducts: []
      }
    },
    components: {
      ProductBox
    },
    mounted() {
      this.getLatestProducts()

      document.title = 'Home | Djackets'
    },
    methods: {
      async getLatestProducts() {
        this.$store.commit('setIsLoading', true)

        await axios.get("/api/v1/latest-products/")
          .then(response => {
            this.latestProducts = response.data
          })
          .catch(error => {
            console.log(error)
          })

        this.$store.commit('setIsLoading', false)
      }
    },
  }
</script>