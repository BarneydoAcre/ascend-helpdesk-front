<template>
    <v-dialog v-model="dialog" max-width="45vw">
        <template v-slot:activator="{ on, attrs }">
            <v-icon small v-bind="attrs" v-on="on" @click="getProductItems">mdi-pencil</v-icon>
        </template>
        <v-card>
            <v-card-title>Componentes do Produto</v-card-title>
            <v-card-text>
                <v-form v-model="valid" ref="form" lazy-validation>
                    <v-row dense>
                        <v-col cols="9">
                            <v-text-field dense filled label="Nome do Produto" @blur="editProduct" v-model="item.name"></v-text-field>
                        </v-col>
                        <v-col cols="3">
                            <v-text-field dense filled label="Preço" @blur="editProduct" v-model="item.price"></v-text-field>
                        </v-col>
                        <v-col cols="12">
                            <h3>Componentes</h3>
                        </v-col>
                        <v-col cols="7">
                            <v-select dense filled label="Componente" v-model="component" @focus="getProduct" :items="products" item-text="name" item-value="id"></v-select>
                        </v-col>
                        <v-col cols="3">
                            <v-text-field dense filled label="Qtde." v-model="quantity"></v-text-field>
                        </v-col>
                        <v-col cols="2">
                            <v-btn fab color="primary" @click="addProductItems">
                                <v-icon>mdi-plus</v-icon>
                            </v-btn>
                        </v-col>
                    </v-row>
                </v-form>
                <div style="max-height: 45vh; overflow-y: auto;">
                    <v-data-table
                    dense 
                    class="elevation-1" 
                    :headers="headers" 
                    :items-per-page="-1" 
                    :items="productsTable" 
                    hide-default-footer>
                        <template v-slot:item.actions="{ item }">
                            <v-icon small @click="item">mdi-delete</v-icon>
                        </template>
                    </v-data-table>
                </div>
            </v-card-text>
            <v-card-actions class="d-flex justify-center align-center">
                <v-btn color="success" @click="0">
                    Salvar Receita
                </v-btn>
            </v-card-actions>
        </v-card>
    </v-dialog>
  
</template>

<script>
export default {
    name: "EditProductSale",
    props: ["product", "item"],
    data () {
        return {
            dialog: false,
            valid: false,
            products: [],
            productsTable: [],
            component: null,
            quantity: null,
            formProduct: {
                company: localStorage.getItem("company"),
                company_worker: localStorage.getItem("user_id"),
                id: this.item.id,       
                name: null,
                price: null,
                type: 2,
            },
            formProductItems: {
                company: localStorage.getItem("company"),
                company_worker: localStorage.getItem("user_id"),
                id: this.item.id,       
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
                { text: "Qtde.", value: "quantity" },
                { text: "Preço", value: "cost" },
                { text: "Ações", value: "actions" },
            ],
        }
    },
    mounted () {
        this.defineConfig()
    },
    methods: {
        defineConfig () {
            this.item["company"] = localStorage.getItem("company")
            this.item["company_worker"] = localStorage.getItem("user_id")
            this.item["type"] = this.formProduct.type
        },
        async getProduct () {
            const req = await fetch(process.env.HOST_BACK+'/foodservice/getProduct/?'+new URLSearchParams({
                token: localStorage.getItem('refresh'),
                company: localStorage.getItem('company'),
                type: 1,
            }), {
                method: "GET",
            })
            const res = await req.json()
            this.products = res
        },
        async getProductItems () {
            const req = await fetch(process.env.HOST_BACK+'/foodservice/getProductItems/?'+new URLSearchParams({
                token: localStorage.getItem('refresh'),
                company: localStorage.getItem('company'),
                type: 1,
                product: this.item.id
            }), {
                method: "GET",
            })
            const res = await req.json()
            this.productsTable = res
        },
        async editProduct () {
            const req = await fetch(process.env.HOST_BACK+'/foodservice/editProduct/?', {
                method: "POST",
                body: JSON.stringify(this.item)
            })
            if (req.status == 200) {

            }
        },
        async addProductItems () {
            const req = await fetch(process.env.HOST_BACK+'/foodservice/addProductItem/?', {
                method: "POST",
                body: JSON.stringify({
                    company: this.formProduct.company,
                    company: this.formProduct.company_worker,
                    id: this.item.id,


                    quantity: this.quantity,
                })
            })
            if (req.status == 200) {
            }
        },
        async removeProductItems () {
            const req = await fetch(process.env.HOST_BACK+'/foodservice/removeProductItems/?', {
                method: "POST",
                body: JSON.stringify({
                    id: this.component,
                })
            })
            const res = await req.json()
            if (req.status == 200) {

            }
        },
    }
}
</script>
