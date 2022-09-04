<template>
    <v-dialog v-model="dialog" max-width="85vw">
        <template v-slot:activator="{ on, attrs }">
            <v-btn
            color="primary"
            width="10vw" 
            v-bind="attrs" 
            v-on="on">
                <v-icon>mdi-office-building-plus</v-icon> Empresa
            </v-btn>
        </template>
        <v-card>
            <v-card-title>
                <h3>Nova Empresa</h3>
            </v-card-title>
            <v-card-text>
                <v-form v-model="valid" ref="form" lazy-validation>
                    <v-container>
                        <v-row dense>
                            <v-col dense cols="12" md="12">
                                <v-text-field dense filled required v-model="form.corporate_name" :rules="[v => !!v || 'Obrigatório']" label="Nome da Empresa"></v-text-field>
                            </v-col>
                            <v-col dense cols="6">
                                <v-text-field dense filled required v-model="form.corporate_cnpj" :rules="textRules" label="CNPJ"></v-text-field>
                            </v-col>
                            <v-col dense cols="6">
                                <v-text-field dense filled required v-model="form.corporate_ie" :rules="textRules" label="IE"></v-text-field>
                            </v-col>
                            <v-col dense cols="6">
                                <v-text-field dense filled required v-model="form.corporate_email" :rules="emailRules" label="Email"></v-text-field>
                            </v-col>
                            <v-col dense cols="6">
                                <v-text-field dense filled required v-model="form.corporate_number" :rules="textRules" label="Telefone ou Celular"></v-text-field>
                            </v-col>
                            <v-col align="center" cols="12" md="12" no-gutter>
                                <v-btn :loading="loadingSave" :disabled="!valid" color="success" class="mr-4" @click="validate">Salvar</v-btn>
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
                company: null,
                company_worker: null,
                corporate_name: '',
                corporate_cnpj: '',
                corporate_ie: '',
                corporate_email: '',
                corporate_number: '',
            },
            textRules: [
                v => !!v || 'Obrigatório',
                v => (v && v.length >= 10) || 'Ao menos 10 caracteres',
            ],
            emailRules: [
                v => !!v || 'Obrigatório',
                v => /.+@.+/.test(v) || 'E-mail inválido',
            ],
            company: '',
            companyPerson: '',
            type: '',
            companys: [],
            customers: [],
            types: []
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
                this.addCustomerCompany()
            },10)
        },
        addCustomerCompany () {
            this.loadingSave = true
            if (this.valid == false) {
                return null
            }
            setTimeout(async () => {
                this.form.company = localStorage.getItem('company')
                this.form.company_worker = localStorage.getItem('user_id')
                const req = await fetch(process.env.HOST_BACK+'/addCustomerCompany/', {
                    method: 'POST',
                    body: JSON.stringify(this.form),
                    headers: {'Content-Type': 'application/json'},
                })
                this.reset()
                this.dialog = false
                this.loadingSave = false
            }, 1500)
        },  
    }
}
</script>