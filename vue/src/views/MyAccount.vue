<template>
    <div class="row mb-5">
        <div class="col-lg-12">
            <h1 class="h2">My Account</h1>
        </div>
        <div class="col-lg-12">
            <button @click="logout" class="btn btn-danger">Log Out</button>
        </div>

        <div class="col-lg-12 mt-3" v-if="orders.length">
            <h2 class="h3">My orders history</h2>

            <OrderSummary v-for="order in orders" v-bind:key="order.id" v-bind:order="order" />
        </div>
    </div>
</template>

<script>
import axios from 'axios'

import OrderSummary from '@/components/OrderSummary.vue'

export default {
    name: "MyAccount",
    components: {
        OrderSummary    
    },
    mounted () {
        document.title = "My account | Djackets"

        this.getMyOrders()
    },
    data() {
        return {
            orders: []
        }
    },
    methods: {
        logout() {
            axios.defaults.headers.common["Authorization"] = ""
            localStorage.removeItem("token")
            localStorage.removeItem("username")
            localStorage.removeItem("userid")            

            this.$store.commit("removeToken")

            this.$router.push("/")
        },
        async getMyOrders() {
            this.$store.commit("setIsLoading", true)

            await axios.get('/api/v1/orders/')
            .then(response => {
                this.orders = response.data
            })
            .catch(error => {
                console.log(error)
            })

            this.$store.commit("setIsLoading", false)
        }
    }
}
</script>