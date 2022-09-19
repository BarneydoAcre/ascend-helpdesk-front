<template>
    <div class="main-foodservice-product">
        <v-data-table fixed-header height="42vh" :loading="loadingTable" :headers="headers" :items="products" :items-per-page="-1" hide-default-footer class="elevation-1"></v-data-table>
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
            loadingTable: false,
            products: [],
            headers: [
                { text: 'ID', justify: 'center', align: 'center', value: 'id' },
                { text: 'Produto', align: "center", justify: "center", value: 'name' },
                { text: 'Marca', align: "center", justify: "center", value: 'brand' },
                { text: 'Custo', align: "center", justify: "center", value: 'cost' },
                { text: 'Qtde.', align: "center", justify: "center", value: 'stock' },
                { text: 'Ações', align: "center", justify: "center", value: 'actions' },
            ]
        }
    },
    mounted () {
        this.getProduct()
    },
    methods: {
        editProduct () {
        },
        async getProduct () {
            this.loadingTable = true
            const req = await fetch(process.env.HOST_BACK+'/foodservice/getProduct/?'+new URLSearchParams({
                token: localStorage.getItem('refresh'),
                company: localStorage.getItem('company'),
                type: 1,
            }), {
                method: "GET",
            })
            const res = await req.json()
            this.loadingTable = false
            this.products = res
        }
    }
}
</script>

<style>
.main-foodservice-product {
    grid-area: product;

    padding: 1vh 1vh 1vh 0;
}
</style>