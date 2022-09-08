<template>
    <div class="main-foodservice-products">
        <v-card min-height="100%">
            <v-card-text v-for="p, i in products" :key="i">
                <EditProduct :product="p"></EditProduct>
            </v-card-text>
        </v-card>
    </div>
</template>

<script>
import EditProduct from "./actions/EditProduct.vue";
export default {
    name: "Product",
    components: {
        EditProduct,
    },
    data () {
        return {
            form: {},
            products: [],
        }
    },
    mounted () {
        this.getProducts()
    },
    methods: {
        editProduct () {
        },
        async getProducts () {
            const req = await fetch(process.env.HOST_BACK+'/foodservice/getProduct/?'+new URLSearchParams({
                token: localStorage.getItem('refresh'),
                company: localStorage.getItem('company'),
            }), {
                method: "GET",
            })
            const res = await req.json()
            this.products = res
        }
    }
}
</script>

<style>
.main-foodservice-products {
    grid-area: products;

    overflow-y: auto;
    padding: 1vh 1vh 0 0;
}
</style>