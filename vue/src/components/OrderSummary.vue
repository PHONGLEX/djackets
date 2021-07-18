<template>
    <div class="table-responsive text-nowrap">
        <table class="table">
            <thead>
                <tr>
                    <th scope="col">#</th>
                    <th scope="col">Item title</th>
                    <th scope="col">Price</th>
                    <th scope="col">Quantity</th>
                    <th scope="col">Total Item Price</th>
                    <th scope="col">Time</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="item in order.items" :key="item.product.id">
                    <td>{{order.id}}</td>
                    <td>{{item.product.name}}</td>
                    <td>{{item.product.price}}</td>
                    <td>{{item.quantity}}</td>
                    <td>${{getItemTotal(item).toFixed(2)}}</td>
                    <td>{{order.created_at}}</td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
    export default {
        name: "OrderSummary",
        props: {
            order: Object
        },
        methods: {
            getItemTotal(item) {
                return item.quantity * item.product.price
            },
        },
        filters: {
            displayDateTime: function (value) {
                if (!value) return ''
                value = new Date(value)
                console.log(value)
                return value
            }
        }
    }
</script>