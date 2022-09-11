<template>
    <div class="main-foodservice">
        <Actions @getProduct="$refs.product.getProduct()"></Actions>
        <Product ref="product"></Product>
        <ProductSale ref="productsale"></ProductSale>
    </div>
</template>

<script>
import Actions from "../../components/foodservice/Actions.vue";
import Product from "../../components/foodservice/Product.vue";
import ProductSale from "../../components/foodservice/ProductSale.vue";
export default {
    name: "FoodService",
    components: {
    Product,
    Actions,
    ProductSale
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
    grid-template-columns: 25% 25% 20% 20% 10%;
    grid-template-rows: 9vh 29vh 9vh 19vh 29vh;
    grid-template-areas: 'actions actions graphs graphs graphs'
                         'product productsale none none none'
                         'product productsale table table table'
                         'product productsale table table table'
                         'product productsale table table table';
}
</style>
