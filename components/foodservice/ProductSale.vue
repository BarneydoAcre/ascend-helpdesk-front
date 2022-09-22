<template>
    <div class="main-foodservice-productsale">
        <v-data-table fixed-header height="42vh" :loading="loadingTable" :headers="headers" :items="products" :items-per-page="-1" hide-default-footer class="elevation-1">
            <template v-slot:item.actions="{ item }">
                <EditProductSale :item="item" :product="products"></EditProductSale>
            </template>
        </v-data-table>
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
            loadingTable: false,
            form: {},
            products: [],
            headers: [
                { text: "ID", align: "center", justify: "center", value: "id" },
                { text: "Produto", align: "center", justify: "center", value: "name" },
                { text: "Custo", align: "center", justify: "center", value: "cost" },
                { text: "Preço", align: "center", justify: "center", value: "price" },
                { text: "Ações", align: "center", justify: "center", value: "actions" },
            ],
        }
    },
    mounted () {
        this.getProductsSale()
    },
    methods: {
        editProduct () {
        },
        async getProductsSale () {
            this.loadingTable = true
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
            this.loadingTable = false
        }
    }
}
</script>

<style>
.main-foodservice-productsale {
    grid-area: productsale;

    padding: 1vh 1vh 0 0;
}
</style>