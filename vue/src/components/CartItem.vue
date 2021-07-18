<template>
    <tr>
        <td>{{item.product.id}}</td>
        <td>
            <router-link :to="item.product.get_absolute_url">
                {{item.product.name}}
            </router-link>
        </td>
        <td>{{item.product.price}}</td>
        <td>
            <a @click="decrementQuantity(item)"><i class="fas fa-minus mr-2"></i></a>
            {{item.quantity}}
            <a @click="incrementQuantity(item)"><i class="fas fa-plus ml-2"></i></a>
        </td>
        <td class="d-flex justify-content-between">
            ${{getItemTotal(item).toFixed(2)}}
            <a @click="removeFromCart(item)"><i class="fas fa-trash float-right"></i></a>
        </td>
    </tr>

    <!-- <tr>
                    <td colspan="4"><b>Order Total</b></td>
                    <td colspan="5"><b>$100</b></td>
                </tr>
                <tr>
                    <td colspan="5">
                        <a class="btn btn-warning float-right me-2" href="">Proceed to
                            checkout</a>
                        <a class="btn btn-primary float-right" href="">Continue shopping</a>
                    </td>

                </tr> -->
</template>

<script>
    export default {
        name: 'CartItem',
        props: {
            initialItem: Object
        },
        data() {
            return {
                item: this.initialItem
            }
        },
        methods: {
            getItemTotal(item) {
                return item.quantity * item.product.price
            },
            decrementQuantity(item) {
                item.quantity -= 1

                if (item.quantity === 0) {
                    this.$emit('removeFromCart', item)
                }

                this.updateCart()
            },
            incrementQuantity(item) {
                item.quantity += 1

                this.updateCart()
            },
            updateCart() {
                localStorage.setItem('cart', JSON.stringify(this.$store.state.cart))
            },
            removeFromCart(item) {
                this.$emit('removeFromCart', item)

                this.updateCart()
            }
        }
    }
</script>