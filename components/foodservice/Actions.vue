<template>
    <div class="main-foodservice-actions">
        <v-card height="100%" class="d-flex justify-space-around align-center">
            <AddProduct @getProducts="$emit('getProducts')"></AddProduct>
            <AddMontage></AddMontage>
        </v-card>
    </div>
    
</template>

<script>
import AddProduct from './actions/AddProduct.vue';
import AddMontage from './actions/AddMontage.vue';
export default {
    name: "Actions",
    emits: ['getProducts',],
    components: {
    AddProduct,
    AddMontage
},
}
</script>

<style>
.main-foodservice-actions {
    grid-area: actions;

    padding: 0 1vh 1vh 0;
}

</style>