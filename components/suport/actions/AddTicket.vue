<template>
    <v-dialog v-model="dialog" max-width="85vw">
        <template v-slot:activator="{ on, attrs }">
            <v-btn 
            tile 
            color="primary"
            width="10vw" 
            v-bind="attrs" 
            v-on="on"
            @click="getCompanys">
                <v-icon>mdi-ticket-account</v-icon> Ticket
            </v-btn>
        </template>
        <v-card>
            <v-card-title>
                <h3>Novo Ticket</h3>
            </v-card-title>
            <v-card-text>
                <v-form v-model="valid" ref="form" lazy-validation>
                  <v-container>
                    <v-row dense>
                      <v-col dense cols="12" md="12">
                        <v-text-field dense filled required v-model="form.title" :rules="textRules" label="Título do Ticket"></v-text-field>
                      </v-col>
                      <v-col cols="6" sm="6" md="6">
                        <v-select dense filled required @blur="getCustomers(form.corporate)" v-model="form.corporate" :rules="selectRules" label="Empresa" :items="companys" item-text="corporate_name" item-value="corporate_id"></v-select>
                      </v-col>
                      <v-col cols="6" sm="6" md="6" class="col-6">
                        <v-select dense filled required v-model="form.customer" :rules="selectRules" label="Solicitante" :items="customers" item-text="customer_name" item-value="customer_id"></v-select>
                      </v-col>
                      <v-col cols="6" sm="4" md="4" >
                        <v-select dense filled required v-model="form.type" :rules="selectRules" label="Tipo de Solicitação" :items="types" item-text="type_name" item-value="type_id"></v-select>
                      </v-col>
                      <v-col cols="6" sm="6" md="6" >
                        <v-text-field dense filled required v-model="form.routine" :rules="textRules" label="Rotina" :items="types" item-text="type" item-value="id"></v-text-field>
                      </v-col>
                      <v-col cols="12" sm="2" md="2">
                        <v-checkbox dense filled required v-model="form.duty" label="Plantão"></v-checkbox>
                      </v-col>  
                      <v-col cols="12" md="12">
                        <v-textarea dense filled required v-model="form.description_problem" :rules="textRules" label="Descrição do problema" rows="2"></v-textarea>
                      </v-col>
                      <v-col align="center" cols="12" md="12" no-gutter>
                        <v-btn :disabled="!valid" color="success" class="mr-4" @click="addTicket">Salvar</v-btn>
                        <v-btn color="warning" class="mr-4" @click="close">Cancelar</v-btn>
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
            dialog: false,
            valid: true,
            ticketError: '',
            form: {
              company: null,
              company_worker: null,
              title: '',
              corporate: '',
              customer: '',
              type: '',
              routine: '',
              duty: false,
              description_problem: '',
              status: 1,
            },
            textRules: [
              v => !!v || 'Obrigatório',
              v => (v && v.length >= 10) || 'Ao menos 10 caracteres',
            ],
            selectRules: [
              v => !!v || 'Obrigatório',
            ],
            email: '',
            emailRules: [
              v => !!v || 'E-mail is required',
              v => /.+@.+/.test(v) || 'E-mail must be valid',
            ],
            company: '',
            companyPerson: '',
            type: '',
            companys: [],
            customers: [],
            types: []
        }
    },
    mounted () {
        this.getCompanys()
        this.getRequestTypes()
        // this.validate()
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
        },
        async addTicket () {
            this.validate()
            if (this.valid) {
                this.form.company = localStorage.getItem('company')
                this.form.company_worker = localStorage.getItem('user_id')
                const req = fetch(process.env.HOST_BACK+'/addTicket/',{
                    method: 'POST',
                    body: JSON.stringify(this.form),
                    headers: {'Content-Type': 'application/json'},
                })
                const res = await req
                if (res.status == 200) {
                    this.reset()
                    this.$emit('getTickets')
                    this.dialog = false
                }else {
                    this.ticketError = "Erro ao registrar: " + res.status
                }
            }else {
            }
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
        async getCustomers (corporate) {
            if (this.form.corporate != '') {
                const req = await fetch(process.env.HOST_BACK+'/getCustomerCompanyWorkers/?'+ new URLSearchParams({
                    company: localStorage.getItem('company'),
                    corporate: corporate,
                    token: localStorage.getItem('refresh')
                }),{
                    method: 'GET',
                })
                const res = await req.json()
                this.customers = res
            }
        },
        async getRequestTypes () {
            const req = await fetch(process.env.HOST_BACK+'/getRequestTypes/?'+ new URLSearchParams({
                company: localStorage.getItem('company'),
                token: localStorage.getItem('refresh')
            }),{
                method: 'GET',
            })
            const res = await req.json()
            this.types = res
        },
    }
}
</script>