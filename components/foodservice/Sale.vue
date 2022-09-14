<template>
    <div class="main-foodservice-sale">
        <v-card min-height="100%" class="grid-sale">
            <v-card-title style="grid-area: title;">Caixa</v-card-title>
            <v-card-text style="grid-area: input;">
                <v-form v-model="valid" ref="form" lazy-validation>
                    <v-row dense>
                        <v-col cols="7">
                            <v-select dense :loading="loadingSelect" filled label="Prato" v-model="product" @click="getProduct" :rules="rules" :items="products" item-text="name" item-value="id"></v-select>
                        </v-col>
                        <v-col cols="3">
                            <v-text-field dense filled label="Qtde." v-model="quantity" :rules="rules"></v-text-field>
                        </v-col>
                        <v-col cols="2">
                            <v-btn fab color="primary" @click="validate">
                                <v-icon>mdi-plus</v-icon>
                            </v-btn>
                        </v-col>
                    </v-row>
                </v-form>
            </v-card-text>
            <v-card-text style="grid-area: table;">
                <v-data-table :headers="headers" :items="formItems.products" hide-default-footer class="elevation-1"></v-data-table>
            </v-card-text>
            <v-card-text style="grid-area: actions;" class="d-flex flex-column justify-start align-center">
                <v-btn fab small color="success" class="mb-4" @click="addSale">
                    <v-icon>mdi-check</v-icon>
                </v-btn>
                <v-btn fab small color="primary">
                    <v-icon>mdi-printer</v-icon>
                </v-btn>
            </v-card-text>
            <v-card-actions style="grid-area: info;">
                <v-row dense class="d-flex align-center">
                    <v-col cols="3">
                        <v-checkbox dense filled label="Frete?" v-model="form.delivery"></v-checkbox>
                    </v-col>
                    <v-col cols="4">
                        <v-text-field dense label="Valor" type="number" v-model="form.delivery_cost"></v-text-field>
                    </v-col>
                </v-row>
            </v-card-actions>
        </v-card>
    </div>
    
</template>

<script>
export default {
    name: "Sale",
    components: {
    },
    data () {
        return {
            valid: false,
            products: [],
            product: null,
            quantity: null,
            loadingSelect: false,
            form: {
                company: localStorage.getItem("company"),
                company_worker: localStorage.getItem("user_id"),
                value: 0.0,
                delivery: false,
                delivery_cost: 0,
                total: 0.0,
            },
            formItems: {
                company: localStorage.getItem("company"),
                company_worker: localStorage.getItem("user_id"),
                sale: null,
                products: [],
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
                { text: "Ações", value: "actions" },
            ],
            rules: [
                v => !!v || "Obrigatório",
            ],
        }
    },
    methods: {
        validate() {
            this.$refs.form.validate()
            setTimeout(() => {
                if (this.valid == true) {
                    this.addItem()
                    this.$refs.form.reset()
                }
            }, 0)
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
        addItem () {
            let objItem = null
            if (this.formItems.products.filter((i) => {return i.id == this.product}).length == 0) {
                objItem = this.products.filter((i) => {return i.id == this.product})[0]
                objItem["quantity"] = this.quantity
                this.formItems.products.push(objItem)
            }
        },
        delItem (item) {
            this.formItems.products = this.formItems.products.filter((i) => {return i.id != item.id}).length == 0
        },
        async addSale () {
            for (let i = 0; this.formItems.products.length > i; i++) {
                this.form.value = parseFloat(this.form.value) + parseFloat(this.formItems.products[i].price)
            }
            this.form.total = this.form.value + this.form.delivery_cost
            const req = await fetch(process.env.HOST_BACK+"/foodservice/addSale/", {
                method: "POST",
                body: JSON.stringify(this.form),
                headers: { "Content-Type": "application/json" }
            })
            this.formItems.sale = await req.json()
            if (req.status == 200) {
                this.addSaleItems()
            }
        },
        async addSaleItems () {
            const req = await fetch(process.env.HOST_BACK+"/foodservice/addSaleItems/", {
                method: "POST",
                body: JSON.stringify(this.formItems),
                headers: { "Content-Type": "application/json" }
            })
            if (req.status == 200) {
                console.log('ok')
            }
        }
    }
}
</script>

<style scoped>
.main-foodservice-sale {
    grid-area: sale;

    padding: 1vh 0 0 1vh;
}
.grid-sale {
    display: grid;
    grid-template-columns: 85% 15%;
    grid-template-rows: 15% 15% 55% 15%;
    grid-template-areas: 'title title' 
                         'input actions'
                         'table actions'
                         'info none';
}
</style>