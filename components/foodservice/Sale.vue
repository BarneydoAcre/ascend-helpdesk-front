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
                <v-data-table :headers="headers" :items="form.product" hide-default-footer class="elevation-1"></v-data-table>
            </v-card-text>
            <v-card-text style="grid-area: actions;" class="d-flex flex-column justify-start align-center">
                <v-btn fab small color="success" class="mb-4">
                    <v-icon>mdi-check</v-icon>
                </v-btn>
                <v-btn fab small color="primary">
                    <v-icon>mdi-printer</v-icon>
                </v-btn>
            </v-card-text>
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
                product: [],
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
            if (this.form.product.filter((i) => {return i.id == this.product}).length == 0) {
                objItem = this.products.filter((i) => {return i.id == this.product})[0]
                objItem["quantity"] = this.quantity
                this.form.product.push(objItem)
            }
        },
        delItem (item) {
            this.formProductItems.items = this.formProductItems.items.filter((i) => {return i.id != item.id}).length == 0
        },
    }
}
</script>

<style>
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
                         'table none';
}
</style>