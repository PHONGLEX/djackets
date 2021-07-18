<template>

    <div class="row">
        <div class="album py-5">
            <div class="pb-3">
                <h1 class="h3 text-center">{{category.name}}</h1>
            </div>
            <div class="row row-cols-1 row-cols-sm-2 row-cols-md-5 g-3">
                <ProductBox
                  v-for="product in category.products"
                  v-bind:key="product.id"
                  v-bind:product="product"
                 />
            </div>
        </div>
    </div>
</template>

<script>
    import axios from 'axios'
    import {
        toast
    } from 'bulma-toast'

    import ProductBox from '@/components/ProductBox.vue'

    export default {
        name: "Category",
        data() {
            return {
                category: {
                    products: []
                }
            }
        },
        components: {
            ProductBox                                                                        
        },
        mounted() {
            this.getCategory()
        },
        watch: {
            $route(to, from) {
                if (to.name === "Category") {
                    this.getCategory()
                }
            }
        },
        methods: {
            async getCategory() {
                const categorySlug = this.$route.params.category_slug

                this.$store.commit('setIsLoading', true)
                await axios
                    .get(`/api/v1/products/${categorySlug}/`)
                    .then(response => {
                        this.category = response.data

                        document.title = this.category.name + ' | Djackets'
                    })
                    .catch(error => {
                        console.log(error)

                        toast({
                            message: "Something went wrong. Please try again.",
                            type: "is-danger",
                            dismissible: true,
                            pauseOnHover: true,
                            duration: 2000,
                            position: "top-right"
                        })
                    })
                this.$store.commit('setIsLoading', false)
            }
        }
    }
</script>