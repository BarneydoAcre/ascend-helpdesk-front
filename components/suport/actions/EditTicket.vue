<template>
    <v-dialog v-model="dialog" max-width="90vw">
        <template v-slot:activator="{ on, attrs }">
            <v-icon @click="activeEdit(item)" v-bind="attrs" v-on="on" small class="mr-2">mdi-pencil</v-icon>
        </template>
        <v-card>
            <v-container>
                <v-form v-model="valid" ref="form" lazy-validation>
                    <v-row dense class="editGrid">
                        <v-col cols="12" style="grid-area: title;">
                            <v-text-field disabled filled dense label="Título do Ticket" v-model="editedItem.title"></v-text-field>
                        </v-col>
                        <v-col cols="12" style="grid-area: corporate;">
                            <v-select disabled filled dense label="Empresa" @blur="getCustomers(editedItem.corporate_id)" v-model="editedItem.corporate_id" :items="companys" item-text="corporate_name" item-value="corporate_id"></v-select>
                        </v-col>
                        <v-col cols="12" style="grid-area: comments;">
                            <v-card height="55vh" style="overflow-y: scroll;">
                                <v-card class="ma-2" v-for="c,i in comments" :key="i">
                                    <v-card-text>{{ c.comment }}</v-card-text>
                                    <v-card-actions>
                                        <h6>{{ c.created }}</h6>
                                        <v-spacer></v-spacer>
                                        <h6>{{ c.worker }}</h6>
                                    </v-card-actions>
                                </v-card>
                            </v-card>
                        </v-col>
                        <v-col cols="12" style="grid-area: addcomment;">
                            <v-row dense no-gutters>
                                <v-col cols="9">
                                    <v-textarea filled dense rows="2" :rules="textRules" v-model="comment" label="Comentário"></v-textarea>
                                </v-col>
                                <v-col cols="3" class="d-flex flex-wrap justify-center">
                                    <v-btn color="success" :loading="loadingComment" @click="validate(comment)">Comentário</v-btn>
                                    <v-btn color="warning" @click="closeTicket(editedItem.id)">Finalizar</v-btn>
                                </v-col>
                            </v-row>
                        </v-col>
                        <v-col style="grid-area: data;">
                            <v-row dense no-gutters>
                                <v-col cols="12">
                                    <v-select disabled filled dense label="Solicitante" v-model="editedItem.customer_id" :items="customers" item-text="customer_name" item-value="customer_id"></v-select>
                                </v-col>
                                <v-col cols="12">
                                    <v-select disabled filled dense label="Tipo" v-model="editedItem.type_id" :items="types" item-text="type_name" item-value="type_id"></v-select>
                                </v-col>
                                <v-col cols="12">
                                    <v-text-field disabled filled dense label="Rotina" v-model="editedItem.routine"></v-text-field>
                                </v-col>
                                <v-col cols="12">
                                    <v-select disabled filled dense label="Status" v-model="editedItem.status_id" :items="status" item-text="status_name" item-value="status_id"></v-select>
                                </v-col>
                                <v-col cols="12">
                                    <v-textarea disabled filled dense rows="3" label="Comentário" v-model="editedItem.description_problem"></v-textarea>
                                </v-col>
                            </v-row>
                        </v-col>
                    </v-row>
                </v-form>
            </v-container>
        </v-card>
    </v-dialog>
</template>

<script>
export default {
    name: 'EditTicket',
    props: ['item'],
    emits: ['getTickets'],
    data () {
        return {
            loadingComment: false,
            valid: true,
            dialog: false,
            editedItem: [],
            textRules: [
              v => !!v || 'Obrigatório',
              v => (v && v.length >= 10) || 'Ao menos 10 caracteres',
            ],
            selectRules: [
              v => !!v || 'Obrigatório',
            ],
            companys: [],
            customers: [],
            types: [],
            status: [],
            comment: '',
            comments: [],
        }
    },
    mounted () {
        this.getCompanys()
        this.getRequestType()
    },
    methods: {
        resetValidation () {
            this.$refs.form.resetValidation()
        },
        validate(comment) {
            this.$refs.form.validate()
            setTimeout(() => {
                this.addComment(comment)
            },10)
        },
        close () {
            this.dialog = false
        },
        activeEdit (item) {
            this.editedItem = Object.assign({}, item)
            this.getTicketStatus()
            this.getCompanys()
            this.getCustomers(this.editedItem.corporate_id)
            this.getComments(item.id)
        },
        async addComment (comment) {
            if (comment == '') {
                return null
            }
            if (this.valid == false) {
                return null
            } 
            this.loadingComment = true
            let data = {
                ticket: this.editedItem.id,
                worker_comment: localStorage.getItem('user_id'),
                comment: comment,
                token: localStorage.getItem('refresh'),
                company: localStorage.getItem('company'),
            }
            const req = await fetch(process.env.HOST_BACK+'/addTicketComment/', {
                method: 'POST',
                body: JSON.stringify(data),
                headers: {'Content-Type': 'application/json'}
            })
            setTimeout(() => {
                    let date = new Date()
                    this.comments.push({
                        comment: comment,
                        created: date.getHours()+':'+date.getMinutes(),
                        worker: localStorage.getItem('username')
                    })
                    this.loadingComment = false
                    this.comment = ''
                    this.resetValidation()
            },1000);
        },
        async getComments (id) {
            const req = await fetch(process.env.HOST_BACK+'/getTicketComments/?'+ new URLSearchParams({
                company: localStorage.getItem('company'),
                ticket: id,
                token: localStorage.getItem('refresh')
            }), {
                method: 'GET',
            })
            const res = await req.json()
            this.comments = res
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
        async getCustomers (corporate_id) {
            if (this.editedItem.corporate == '') {
              
            }else {
                const req = await fetch(process.env.HOST_BACK+'/getCustomerCompanyWorkers/?'+ new URLSearchParams({
                    company: localStorage.getItem('company'),
                    corporate: corporate_id,
                    token: localStorage.getItem('refresh')
                }),{
                    method: 'GET',
                })
                const res = await req.json()
                this.customers = res
            }
        },
        async getRequestType () {
            const req = await fetch(process.env.HOST_BACK+'/getRequestType/?'+ new URLSearchParams({
                company: localStorage.getItem('company'),
                token: localStorage.getItem('refresh')
            }),{
                method: 'GET',
            })
            const res = await req.json()
            this.types = res
        },
        async getTicketStatus () {
            const req = await fetch(process.env.HOST_BACK+'/getTicketStatus/?'+ new URLSearchParams({
                company: localStorage.getItem('company'),
                token: localStorage.getItem('refresh')
            }),{
                method: 'GET',
            })
            const res = await req.json()
            this.status = res
        },
        async closeTicket (id) {
            const req = await fetch(process.env.HOST_BACK+'/closeTicket/', {
                method: 'POST',
                body: JSON.stringify({id: id}),
                headers: {'Content-Type': 'application/json'},
            })
            if (req.status == 200) {
                this.$emit('getTickets')
                this.dialog = false
            }
        },
    }
}
</script>

<style scoped>
.editGrid {
    display: grid;
    grid-template-columns: 75% 25%;
    grid-template-rows: 12vh 55vh 17vh;
    grid-gap: 1vh;
    grid-template-areas:  'title corporate'
                          'comments data'
                          'addcomment data';
}
</style>