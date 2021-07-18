<template>
    <div class="container dark-grey-text mt-5">

      <!--Grid row-->
      <div class="row wow fadeIn">

        <!--Grid column-->
        <div class="col-md-6 mt-4">

          <img :src="product.get_image" class="img-fluid" alt="">

        </div>
        <!--Grid column-->

        <!--Grid column-->
        <div class="col-md-6 mb-4">

          <!--Content-->
          <div class="p-4">

            <div class="mb-3">
              <a href="">
                <span class="badge purple mr-1">{{product.category?.name}}</span>
              </a>
            </div>

            <p class="lead">
              <span>${{product.price}}</span>
            </p>

            <p class="lead font-weight-bold">{{product.name}}</p>

            <p>{{product.description}}</p>

            <form class="d-flex justify-content-left">
              <!-- Default input -->
              <input type="number" min="1" v-model="quantity" aria-label="Search" class="form-control me-1" style="width: 100px">
              <a class="btn btn-primary btn-md my-0 p waves-effect waves-light" @click="addToCart">Add to cart
                <i class="fas fa-shopping-cart ml-1"></i>
              </a>

            </form>

          </div>
          <!--Content-->
        </div>
        <!--Grid column-->

      </div>
      <!--Grid row-->

      <hr>

      <!--Grid row-->
      <div class="row d-flex justify-content-center wow fadeIn">

        <!--Grid column-->
        <div class="col-md-6 text-center">

          <h4 class="my-4 h4">Additional information</h4>

          <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Natus suscipit modi sapiente illo soluta odit
            voluptates,
            quibusdam officia. Neque quibusdam quas a quis porro? Molestias illo neque eum in laborum.</p>

        </div>
        <!--Grid column-->

      </div>
      <!--Grid row-->

      <!--Grid row-->
      <div class="row wow fadeIn">

        <!--Grid column-->
        <div class="col-lg-4 col-md-12 mb-4">

          <img src="https://mdbootstrap.com/img/Photos/Horizontal/E-commerce/Products/11.jpg" class="img-fluid" alt="">

        </div>
        <!--Grid column-->

        <!--Grid column-->
        <div class="col-lg-4 col-md-6 mb-4">

          <img src="https://mdbootstrap.com/img/Photos/Horizontal/E-commerce/Products/12.jpg" class="img-fluid" alt="">

        </div>
        <!--Grid column-->

        <!--Grid column-->
        <div class="col-lg-4 col-md-6 mb-4">

          <img src="https://mdbootstrap.com/img/Photos/Horizontal/E-commerce/Products/13.jpg" class="img-fluid" alt="">

        </div>
        <!--Grid column-->

      </div>
      <!--Grid row-->

    </div>
</template>

<script>
import axios from 'axios'
import { toast } from 'bulma-toast'

export default {
    name: "Product",
    data() {
        return {
            product: {},
            quantity: 1
        }
    },
    mounted() {
        this.getProduct()
    },
    methods: {
        async getProduct() {
            this.$store.commit('setIsLoading', true)

            const category_slug = this.$route.params.category_slug
            const product_slug = this.$route.params.product_slug

            await axios
            .get(`/api/v1/products/${category_slug}/${product_slug}`)
            .then(response => {
                this.product = response.data

                document.title = this.product.name + ' | Djackets'
            })
            .catch(error => console.log(error))

            this.$store.commit('setIsLoading', false)
        },
        addToCart() {
          if (isNaN(this.quantity) || this.quantity < 1) {
            this.quantity = 1
          }

          const item = {
            product: this.product,
            quantity: parseInt(this.quantity)
          }
          
          this.$store.commit('addToCart', item)

          toast({
                message: 'The product was added to the cart',
                type: 'is-success',
                dismissible: true,
                pauseOnHover: true,
                duration: 2000,
                position: 'bottom-right',
            })
        }
    }
}
</script>