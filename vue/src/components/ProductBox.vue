<template>
    <div class="col">
        <div class="card border-0">
            <img class="img-fluid" alt="Responsive image" :src="product.get_image">

            <div class="card-body px-5">
                <p class="card-text">
                    <router-link v-bind:to="product.get_absolute_url" class="text-dark text-decoration-none" href="">{{ product.name }}</router-link>
                </p>
                <div class="fw-bold">£{{product.price}}</div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'ProductBox',
        props: {
            product: Object
        }
    }
</script>