<template>
    <div class="main-foodservice-productsale">
        <v-card min-height="100%">
            <v-card-text v-for="p, i in products" :key="i">
                <EditProductSale :product="p"></EditProductSale>
            </v-card-text>
        </v-card>
    </div>
</template>

<script>
import EditProductSale from "./actions/EditProductSale.vue";
export default {
    name: "Product",
    components: {
        EditProductSale,
    },
    data () {
        return {
            form: {},
            products: [],
        }
    },
    mounted () {
        this.getProductsSale()
    },
    methods: {
        editProduct () {
        },
        async getProductsSale () {
            const req = await fetch(process.env.HOST_BACK+'/foodservice/getProduct/?'+new URLSearchParams({
                token: localStorage.getItem('refresh'),
                company: localStorage.getItem('company'),
                company_worker: localStorage.getItem('user_id'),
                type: 2,
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
.main-foodservice-productsale {
    grid-area: productsale;

    overflow-y: auto;
    padding: 1vh 1vh 0 1vh;
}
</style>