<template>
    <div class="main-foodservice">
        <Actions @getProducts="$refs.product.getProducts()"></Actions>
        <Product ref="product"></Product>
    </div>
</template>

<script>
import Actions from "../../components/foodservice/Actions.vue";
import Product from "../../components/foodservice/Product.vue";
export default {
    name: "FoodService",
    components: { 
        Product,
        Actions,
    },
    data() {
        return {
            form: {}
        };
    },
    methods: {
    },
}
</script>

<style>
.main-foodservice {
    display: grid;
    grid-template-columns: 25% 20% 20% 20% 15%;
    grid-template-rows: 9vh 29vh 9vh 19vh 29vh;
    grid-template-areas: 'actions graphs graphs graphs graphs'
                         'products none none none none'
                         'products table table table table'
                         'products table table table table'
                         'products table table table table';
}
</style>
