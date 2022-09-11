<template>
    <div class="main-foodservice-actions">
        <v-card height="100%" class="d-flex justify-space-around align-center">
            <AddProduct @getProduct="$emit('getProduct')"></AddProduct>
            <AddProductSale @getProductSale="$emit('getProductSale')"></AddProductSale>
        </v-card>
    </div>
    
</template>

<script>
import AddProduct from './actions/AddProduct.vue';
import AddProductSale from './actions/AddProductSale.vue';
export default {
    name: "Actions",
    emits: ['getProducts',],
    components: {
    AddProduct,
    AddProductSale
},
}
</script>

<style>
.main-foodservice-actions {
    grid-area: actions;

    padding: 0 1vh 1vh 0;
}

</style>