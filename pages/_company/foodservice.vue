<template>
    <div class="main-foodservice">
        <Actions @getProduct="$refs.product.getProduct()" @getProductSale="$refs.productsale.getProductsSale()"></Actions>
        <Product ref="product"></Product>
        <ProductSale ref="productsale"></ProductSale>
        <Sale ref="sale" @getProduct="$refs.product.getProduct()"></Sale>
    </div>
</template>

<script>
import Actions from "../../components/foodservice/Actions.vue";
import Product from "../../components/foodservice/Product.vue";
import ProductSale from "../../components/foodservice/ProductSale.vue";
import Sale from "../../components/foodservice/Sale.vue";
export default {
    name: "FoodService",
    components: {
    Product,
    Actions,
    ProductSale,
    Sale
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
    grid-template-rows: 9vh 14.5vh 29vh 14.5vh 29vh;
    grid-template-areas: 'actions actions graphs graphs graphs'
                         'product product none none none'
                         'product product sale sale sale'
                         'productsale productsale sale sale sale'
                         'productsale productsale sale sale sale';
}
</style>
