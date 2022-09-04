<template>
    <v-dialog max-width="40vw">
        <template v-slot:activator="{ attrs, on }">
            <v-list-item
            color="primary"
            v-bind="attrs" 
            v-on="on" 
            @click="getRequestType">
                <v-list-item-icon>
                    <v-icon>mdi-plus</v-icon>
                </v-list-item-icon>
                <v-list-item-content>
                    Tipo de Solicitação
                </v-list-item-content>
            </v-list-item>
        </template>
        <v-card>
            <v-card-title>Tipo de Solicitação</v-card-title>
            <v-card-text>
                <v-form ref="form" lazy-validation>
                    <v-row dense>
                        <v-col cols="10">
                            <v-text-field dense filled label="Tipo de Solicitação" v-model="form.request_name"></v-text-field>
                        </v-col>
                        <v-col cols="2">
                            <v-btn fab color="primary" @click="addRequestType">
                                <v-icon>mdi-plus</v-icon>
                            </v-btn>
                        </v-col>
                    </v-row>
                </v-form>
                <v-data-table :headers="headers" :items="types" hide-default-footer class="elevation-1">
                    <template v-slot:item.actions="{ item }">
                        <v-icon small @click="deleteRequestType(item.type_id)">mdi-delete</v-icon>
                    </template>
                </v-data-table>
            </v-card-text>
        </v-card>
    </v-dialog>
</template>

<script>
export default {
    name: 'AddRequestType',
    data () {
        return {
            form: {
                company: localStorage.getItem('company'),
                request_name: '',
            },
            headers: [
                {
                    text: 'ID',
                    align: 'center',
                    justify: 'center',
                    sortable: true,
                    value: 'type_id',
                },              
                { text: 'Type', value: 'type_name' },
                { text: 'Opções', value: 'actions', sortable: false },
            ],
            types: [],
        }
    },
    mounted () {
        this.getRequestType()
    },  
    methods: {
        reset () {
            this.$refs.form.reset()
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
        async addRequestType () {
            const req = await fetch(process.env.HOST_BACK+'/addRequestType/', {
                method: 'POST',
                body: JSON.stringify(this.form),
                headers: {'Content-Type': 'application/json'},
            })
            if (req.status == 200) {
                this.reset()
                this.getRequestType()
                this.dialog = false
            }
        },
        async deleteRequestType (id) {
            const req = await fetch(process.env.HOST_BACK+'/deleteRequestType/', {
                method: 'POST',
                body: JSON.stringify({
                    company: this.form.company,
                    id: id
                }),
                headers: {'Content-Type': 'application/json'},
            })
            if (req.status == 200) {
                this.form.status = ''
                this.getRequestType()
            }
        }
    }
}
</script>

<style>

</style>