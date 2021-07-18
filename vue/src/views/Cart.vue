<template>
    <div class="table-responsive text-nowrap">
        <h2 class="h2">Order Summary</h2>
        <table class="table" v-if="cartTotalLength">
            <thead>
                <tr>
                    <th scope="col">#</th>
                    <th scope="col">Item title</th>
                    <th scope="col">Price</th>
                    <th scope="col">Quantity</th>
                    <th scope="col">Total Item Price</th>
                </tr>
            </thead>
            <tbody>
                <CartItem v-for="item in cart.items"
                    :key="item.product.id"
                    :initialItem="item"
                    v-on:removeFromCart="removeFromCart"
                 />
                <tr>
                    <td colspan="4"><b>Order Total</b></td>
                    <td colspan="5"><b>${{cartTotalPrice.toFixed(2)}}, {{cartTotalLength}} items</b></td>
                </tr>
                <tr>
                    <td colspan="5">
                        <div class="d-flex flex-row-reverse">
                            <router-link to="/cart/checkout/" class="btn btn-warning float-right" href="">Proceed to
                                checkout</router-link>
                            <router-link to="/" class="btn btn-primary float-right me-2" href="">Continue shopping</router-link>
                        </div>
                    </td>

                </tr>                
            </tbody>
        </table>
        <p v-else>You dont have any product in your cart...</p>
    </div>
</template>

<script>
import axios from 'axios'
import CartItem from '@/components/CartItem.vue'

export default {
    name: "Cart",
    components: {
        CartItem
    },
    data() {
        return {
            cart: {
                items: []
            }
        }
    },
    mounted() {
        this.cart = this.$store.state.cart
    },
    computed: {
        cartTotalLength() {
            return this.cart.items.reduce((acc, curVal) => {
                return acc += curVal.quantity
            }, 0)
        },
        cartTotalPrice() {
            return this.cart.items.reduce((acc, curVal) => {
                return acc += curVal.product.price * curVal.quantity
            }, 0)
        }
    },
    methods: {
        removeFromCart(item) {
            this.cart.items = this.cart.items.filter(i => i.product.id !== item.product.id)
        }
    }
}
</script>