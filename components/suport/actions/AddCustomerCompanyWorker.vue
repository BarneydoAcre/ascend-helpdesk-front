<template>
    <v-dialog v-model="dialog" max-width="85vw">
        <template v-slot:activator="{ on, attrs }">
            <v-btn 
            color="primary"
            width="10vw" 
            v-bind="attrs" 
            v-on="on"
            @click="getCompanys">
                <v-icon>mdi-account-plus</v-icon> Cliente
            </v-btn>
        </template>
        <v-card>
            <v-card-title>
                <h3>Novo Cliente</h3>
            </v-card-title>
            <v-card-text>
                <v-form v-model="valid" ref="form" lazy-validation>
                    <v-container>
                        <v-row dense>
                            <v-col dense cols="12" md="12">
                                <v-text-field dense filled required v-model="form.customer_name" :rules="[v => !!v || 'Obrigatório']" label="Nome do cliente"></v-text-field>
                            </v-col>
                            <v-col dense cols="6">
                                <v-text-field dense filled required v-model="form.customer_cpf" :rules="textRules" label="CPF"></v-text-field>
                            </v-col>
                            <v-col dense cols="6">
                                <v-text-field dense filled required v-model="form.customer_email" :rules="emailRules" label="Email"></v-text-field>
                            </v-col>
                            <v-col dense cols="4">
                                <v-text-field dense filled required v-model="form.customer_phone" :rules="textRules" label="Contato"></v-text-field>
                            </v-col>
                            <v-col dense cols="4">
                                <v-select dense filled required label="Status" :items="workerStatus" item-text="status" item-value="id"></v-select>
                            </v-col>
                            <v-col dense cols="4">
                                <v-select dense filled required v-model="form.corporate" label="Empresa" :items="companys" item-text="corporate_name" item-value="corporate_id"></v-select>
                            </v-col>
                            <v-col align="center" cols="12" md="12" no-gutter>
                                <v-btn :loading="loadingSave" :disabled="!valid" color="success" class="mr-4" @click="addCustomerCompanyWorker">Salvar</v-btn>
                                <v-btn :loading="loadingSave" color="warning" class="mr-4" @click="close">Cancelar</v-btn>
                            </v-col>
                        </v-row>
                    </v-container>
                </v-form>
            </v-card-text>
        </v-card>
    </v-dialog>
</template>

<script>
export default {
    name: 'AddTicket',
    emits: ['getTickets'],
    data () {
        return {
            loadingSave: false,
            dialog: false,
            valid: true,
            form: {
                corporate: '',
                customer_name: '',
                customer_cpf: '',
                customer_email: '',
                customer_phone: '',
                customer_admin: false,
                customer_staff: false,
                customer_normal: true,
            },
            workerStatus: [
                { status: 'Admin', id: 1 },
                { status: 'Staff', id: 2 },
                { status: 'Normal', id: 3 },
            ],
            textRules: [
                v => !!v || 'Obrigatório',
                v => (v && v.length >= 10) || 'Ao menos 10 caracteres',
            ],
            emailRules: [
                v => !!v || 'Obrigatório',
                v => /.+@.+/.test(v) || 'E-mail inválido',
            ],
            companys: [],
        }
    },
    methods: {
        close () {
           this.dialog = false
           this.reset()
        },
        reset () {
            this.$refs.form.reset()
        },
        validate () {
            this.$refs.form.validate()
            setTimeout(() => {
                this.addCustomerCompanyWorker()
            },)
        },
        addCustomerCompanyWorker () {
            this.loadingSave = true
            if (this.valid == false) {
                return null
            }
            setTimeout(async () => {
                const req = await fetch(process.env.HOST_BACK+'/addCustomerCompanyWorker/', {
                    method: 'POST',
                    body: JSON.stringify(this.form),
                    headers: {'Content-Type': 'application/json'},
                })
                this.reset()
                this.dialog = false
                this.loadingSave = false
            }, 1500)
        },  
        async getCompanys () {
            const req = await fetch(process.env.HOST_BACK+'/getCustomerCompanys/?'+ new URLSearchParams({
                company: localStorage.getItem('company'),
                token: localStorage.getItem('refresh')
            }),{
                method: 'GET',
            })
            const res = await req.json()
            this.companys = res
        },
    }
}
</script>