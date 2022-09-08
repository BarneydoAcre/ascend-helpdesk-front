<template>
    <v-dialog v-model="dialog" max-width="85vw">
        <template v-slot:activator="{ on, attrs }">
            <v-btn 
            fab
            small
            color="primary" 
            v-bind="attrs" 
            v-on="on">
                <v-icon>mdi-basket-plus</v-icon>
            </v-btn>
        </template>
        <v-card>
            <v-card-title>Novo Produto</v-card-title>
            <v-card-text>
                <v-form v-model="valid" ref="form" lazy-validation>
                    <v-row dense>
                        <v-col cols="8">
                            <v-text-field dense filled label="Nome do Produto" v-model="form.name"></v-text-field>
                        </v-col>
                        <v-col cols="4">
                            <v-select dense filled label="Marca" v-model="form.brand" :items="brands" item-text="brand_name" item-value="brand_id"></v-select>
                        </v-col>
                        <v-col cols="4">
                            <v-select dense filled label="Und. Medida" v-model="form.measure" :items="measures" item-text="measure_name" item-value="measure_id"></v-select>
                        </v-col>
                        <v-col cols="4">
                            <v-text-field dense filled label="Qtd. Atual" v-model="form.stock"></v-text-field>
                        </v-col>
                        <v-col cols="4">
                            <v-text-field dense filled label="Custo" v-model="form.cost"></v-text-field>
                        </v-col>
                        <v-col cols="12" class="d-flex justify-center align-center">
                            <v-btn color="success" class="mr-4" @click="addProduct">
                                <v-icon>mdi-send</v-icon>Registrar
                            </v-btn>
                            <v-btn color="warning" @click="dialog = false">
                                <v-icon>mdi-close</v-icon>Cancelar
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
    name: "AddProduct",
    emits: ['getProducts'],
    data () {
        return {
            dialog: false,
            valid: false,
            form: {
                company: localStorage.getItem('company'),
                company_worker: localStorage.getItem('user_id'), 
                name: null, 
                brand: null,
                measure: null,
                stock: null,
                cost: null,
            },
            brands: [],
            measures: []
        }
    },
    mounted () {
        this.getBrand()
        this.getMeasure()
    },  
    methods: {
        reset () {
            this.$refs.form.reset()
        },
        validate () {
            this.$refs.form.validate()
        },
        async addProduct () {
            const req = await fetch(process.env.HOST_BACK+'/foodservice/addProduct/',{
                method: "POST",
                body: JSON.stringify(this.form),
                headers: {'Content-Type': 'application/json'},
            })

            if (req.status == 200) {
                this.dialog = false
                this.$emit('getProducts')
            }else {
            }
        },
        async getBrand () {
            const req = await fetch(process.env.HOST_BACK+'/foodservice/getBrand/?'+new URLSearchParams({
                token: localStorage.getItem('refresh'),
                company: localStorage.getItem('company'),
            }), {
                method: "GET",
            })
            const res = await req.json()
            this.brands = res
        },
        async getMeasure () {
            const req = await fetch(process.env.HOST_BACK+'/foodservice/getMeasure/?'+new URLSearchParams({
                token: localStorage.getItem('refresh'),
                company: localStorage.getItem('company'),
            }), {
                method: "GET",
            })
            const res = await req.json()
            this.measures = res
        }
    }
}
</script>
