<template>
    <div class="main-foodservice-actions">
        <v-card height="100%" class="d-flex justify-space-around align-center">
            <AddProduct @getProduct="$emit('getProduct')"></AddProduct>
            <AddProductStock @getProduct="$emit('getProduct')"></AddProductStock>
            <AddProductSale @getProductSale="$emit('getProductSale')"></AddProductSale>
        </v-card>
    </div>
    
</template>

<script>
import AddProduct from './actions/AddProduct.vue';
import AddProductSale from './actions/AddProductSale.vue';
import AddProductStock from './actions/AddProductStock.vue';
export default {
    name: "Actions",
    emits: ['getProducts',],
    components: {
    AddProduct,
    AddProductSale,
    AddProductStock
},
}
</script>

<style>
.main-foodservice-actions {
    grid-area: actions;

    padding: 0 1vh 1vh 0;
}

</style>