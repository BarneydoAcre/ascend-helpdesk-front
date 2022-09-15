<template>
    <v-dialog v-model="dialog" max-width="85vw">
        <template v-slot:activator="{ on, attrs }">
            <v-btn 
            fab
            small
            color="primary" 
            v-bind="attrs" 
            v-on="on">
                <v-icon>mdi-package-variant-plus</v-icon>
            </v-btn>
        </template>
        <v-card>
            <v-card-title>Adicionar Estoque</v-card-title>
            <v-card-text>
                <v-form v-model="valid" ref="form" lazy-validation>
                    <v-row dense>
                        <v-col cols="7">
                            <v-select dense filled label="Produto" @click="getProduct" v-model="form.product" :items="products" item-text="name" item-value="id"></v-select>
                        </v-col>
                        <v-col cols="2">
                            <v-text-field dense filled label="Qtde." type="number" v-model="form.quantity"></v-text-field>
                        </v-col>
                        <v-col cols="2">
                            <v-text-field dense filled label="Preço" type="number" v-model="form.cost"></v-text-field>
                        </v-col>
                        <v-col cols="1">
                            <v-btn fab color="primary" @click="addProductStock">
                                <v-icon>mdi-send</v-icon>
                            </v-btn>
                        </v-col>
                    </v-row>
                </v-form>
            </v-card-text>
        </v-card>
    </v-dialog>
</template>

<script>
export default {
    name: "AddProductStock",
    emits: ["getProduct"],
    data () {
        return {
            dialog: false,
            valid: false,
            form: {
                company: localStorage.getItem("company"),
                company_worker: localStorage.getItem("user_id"),
                product: null,
                quantity: null,
                cost: null,
            },
            products: [],
        }
    }, 
    methods: {
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
        async addProductStock () {
            const req = await fetch(process.env.HOST_BACK+"/foodservice/addProductStock/", {
                method: "POST",
                body: JSON.stringify(this.form),
                headers: { "Content-Type": "application/json" },
            })
            if (req.status == 200) {
                this.$emit('getProduct')
            }
        }
    }

}
</script>