<template>
    <v-dialog v-model="dialog" max-width="45vw">
        <template v-slot:activator="{ on, attrs }">
            <v-btn
            fab
            small
            color="primary" 
            v-bind="attrs" 
            v-on="on">
                    <v-icon>mdi-hamburger-plus</v-icon>
            </v-btn>
        </template>
        <v-card>
            <v-card-title>Componentes do Produto</v-card-title>
            <v-card-text>
                <v-form ref="form" lazy-validation>
                    <v-row dense>
                        <v-col cols="9">
                            <v-text-field dense filled label="Nome do Produto" v-model="formProduct.name"></v-text-field>
                        </v-col>
                        <v-col cols="3">
                            <v-text-field dense filled label="Preço" v-model="formProduct.price"></v-text-field>
                        </v-col>
                        <v-col cols="12">
                            <h3>Componentes</h3>
                        </v-col>
                        <v-col cols="7">
                            <v-select dense filled label="Componente" v-model="component" :items="products" item-text="name" item-value="id"></v-select>
                        </v-col>
                        <v-col cols="3">
                            <v-text-field dense filled label="Qtde." v-model="quantity"></v-text-field>
                        </v-col>
                        <v-col cols="2">
                            <v-btn fab color="primary" @click="addItem">
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
                    :items="formProductItems.items" 
                    hide-default-footer>
                        <template v-slot:item.actions="{ item }">
                            <v-icon small @click="delItem(item)">mdi-delete</v-icon>
                        </template>
                    </v-data-table>
                </div>
            </v-card-text>
            <v-card-actions class="d-flex justify-center align-center">
                <v-btn color="success" @click="addProductSale">
                    Salvar Receita
                </v-btn>
            </v-card-actions>
        </v-card>
    </v-dialog>
</template>

<script>
export default {
    name: "AddProductSale",
    emits: ["getProductSale",],
    data () {
        return {
            component: null,
            quantity: null,
            products: [],
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
            dialog: false,
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
        this.getProduct()
    },
    methods: {
        async addProductSale () {
            const req = await fetch(process.env.HOST_BACK+"/foodservice/addProduct/", {
                method: "POST",
                body: JSON.stringify(this.formProduct),
                headers: { "Content-Type": "application/json"}
            })
            const res = await req.json()
            if (req.status == 200) {
                console.log(res)
                this.formProductItems.product_sale = res
                this.addProductSaleItems()
            }
        },
        async addProductSaleItems () {
            const req = await fetch(process.env.HOST_BACK+"/foodservice/addProductItems/", {
                method: "POST",
                body: JSON.stringify(this.formProductItems),
                headers: { "Content-Type": "application/json"}
            })
            if (req.status == 200) {
                this.dialog = false
                this.$emit('getProductSale')
            }
        },
        addItem () {
            let objItem = null
            if (this.formProductItems.items.filter((i) => {return i.id == this.component}).length == 0) {
                objItem = this.products.filter((i) => {return i.id == this.component})[0]
                objItem["quantity"] = this.quantity
                this.formProductItems.items.push(objItem)
                this.component = null
                this.quantity = null
            }
        },
        delItem (item) {
            this.formProductItems.items = this.formProductItems.items.filter((i) => {return i.id != item.id}).length == 0
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
    }
}
</script>
