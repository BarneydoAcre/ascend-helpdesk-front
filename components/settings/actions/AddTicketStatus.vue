<template>
    <v-dialog max-width="40vw">
        <template v-slot:activator="{ attrs, on }">
            <v-list-item
            color="primary"
            v-bind="attrs" 
            v-on="on" 
            @click="getTicketStatus">
                <v-list-item-icon>
                    <v-icon>mdi-plus</v-icon>
                </v-list-item-icon>
                <v-list-item-content>
                    Status Ticket
                </v-list-item-content>
            </v-list-item>
        </template>
        <v-card>
            <v-card-title>Status dos Tickets</v-card-title>
            <v-card-text>
                <v-form ref="form" lazy-validation>
                    <v-row dense>
                        <v-col cols="10">
                            <v-text-field dense filled label="Nome do Status" v-model="form.status"></v-text-field>
                        </v-col>
                        <v-col cols="2">
                            <v-btn fab color="primary" @click="addTicketStatus">
                                <v-icon>mdi-plus</v-icon>
                            </v-btn>
                        </v-col>
                    </v-row>
                </v-form>
                <v-data-table :headers="headers" :items="status" hide-default-footer class="elevation-1">
                    <template v-slot:item.actions="{ item }">
                        <v-icon small @click="deleteTicketStatus(item.status_id)">mdi-delete</v-icon>
                    </template>
                </v-data-table>
            </v-card-text>
        </v-card>
    </v-dialog>
</template>

<script>
export default {
    name: 'AddTicketStatus',
    data () {
        return {
            form: {
                company: localStorage.getItem('company'),
                status: '',
            },
            headers: [
                {
                    text: 'ID',
                    align: 'center',
                    justify: 'center',
                    sortable: true,
                    value: 'status_id',
                },              
                { text: 'Status', value: 'status_name' },
                { text: 'Opções', value: 'actions', sortable: false },
            ],
            status: [],
        }
    },
    mounted () {
        this.getTicketStatus()
    },  
    methods: {
        reset () {
            this.$refs.form.reset()
        },
        async getTicketStatus () {
            const req = await fetch(process.env.HOST_BACK+'/getTicketStatus/', {
                method: 'GET',
            })
            const res = await req.json()
            this.status = res
        },
        async addTicketStatus () {
            const req = await fetch(process.env.HOST_BACK+'/addTicketStatus/', {
                method: 'POST',
                body: JSON.stringify(this.form),
                headers: {'Content-Type': 'application/json'},
            })
            if (req.status == 200) {
                this.reset()
                this.dialog = false
                this.getTicketStatus()
            }
        },
        async deleteTicketStatus (id) {
            const req = await fetch(process.env.HOST_BACK+'/deleteTicketStatus/', {
                method: 'POST',
                body: JSON.stringify({
                    company: this.form.company,
                    id: id
                }),
                headers: {'Content-Type': 'application/json'},
            })
            if (req.status == 200) {
                this.form.status = ''
                this.getTicketStatus()
            }
        }
    }
}
</script>

<style>

</style>