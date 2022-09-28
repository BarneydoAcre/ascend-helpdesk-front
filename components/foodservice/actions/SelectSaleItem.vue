<template>
    <v-dialog v-model="dialog" max-width="45vw">
        <template v-slot:activator="{ on, attrs }">
            <v-col cols="7"
            color="primary" 
            v-bind="attrs">
                <v-text-field dense filled :loading="loadingSelect" @click="getProduct" @keydown.enter="filterProduct" label="Prato" v-model="product.name" :rules="rules"></v-text-field>
            </v-col>
        </template>
        <v-card>
            <v-card-title>Componentes do Produto</v-card-title>
            <v-card-text>
                <v-form v-model="valid" ref="form" lazy-validation>
                    <v-row dense>
                        <v-col cols="12">
                            <h3>Prato</h3>
                        </v-col>
                        <v-col cols="12">
                            <v-text-field dense filled label="Produto" v-model="product.name" @keyup="filterProduct"></v-text-field>
                        </v-col>
                    </v-row>
                </v-form>
                <div style="max-height: 45vh; overflow-y: auto;">
                    <v-data-table
                    dense 
                    class="elevation-1" 
                    :headers="headers" 
                    :items-per-page="-1" 
                    :items="filtered" 
                    hide-default-footer>
                        <template v-slot:item.actions="{ item }">
                            <v-icon small @click="setItem(item.id)">mdi-plus</v-icon>
                        </template>
                    </v-data-table>
                </div>
            </v-card-text>
        </v-card>
    </v-dialog>
</template>

<script>
export default {
    name: "SelectSaleItem",
    data () {
        return {
            loadingSelect: false,
            valid: false,
            product: { id: null, name: null },
            quantity: null,
            products: [],
            dialog: false,
            filtered: [],
            rules: [
                v => !!v || "Obrigatório",
            ],
            formProduct: {
                company: localStorage.getItem("company"),
                company_worker: localStorage.getItem("user_id"),
                name: null,
                price: null,
                type: 2,
            },
            formProductItems: {
                company: localStorage.getItem("company"),
                company_worker: localStorage.getItem("user_id"),
                product_sale: null,
                items: [],
            },
            headers: [
                { 
                    text: "ID",
                    align: "center",
                    justify: "center",
                    value: "id"
                },
                { text: "Produto", value: "name" },
                { text: "Preço", value: "price" },
                { text: "Ações", value: "actions" },
            ],
        }
    },
    mounted () {
        this.getProduct()
    },
    methods: {
        reset () {
            this.$refs.form.reset();
        },
        async getProduct () {
            this.loadingSelect = true
            const req = await fetch(process.env.HOST_BACK+'/foodservice/getProduct/?'+new URLSearchParams({
                token: localStorage.getItem('refresh'),
                company: localStorage.getItem('company'),
                type: 2,
            }), {
                method: "GET",
            })
            const res = await req.json()
            this.products = res
            this.loadingSelect = false
        },
        filterProduct () {
            if(this.product.name == null || this.product.name == '') {
                this.filtered = this.products
            }else {
                this.filtered = this.products.filter((i) => i.name.toLowerCase().includes(this.product.name.toLowerCase()))
                if (this.filtered.length == 1) {
                    this.setItem(this.filtered[0].id)
                }else {
                    this.dialog = true
                }
            }
        },
        setItem (id) {
            this.product = this.products.filter((i) => {
                return i.id == id
            })[0]
            this.dialog = false
        }    
    }
}
</script>
