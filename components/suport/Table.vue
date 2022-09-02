<template>
    <div id="main-suport-table">
        <v-data-table :headers="headers" :items="tickets" hide-default-footer class="elevation-1">
            <template v-slot:item.actions="{ item }">
                <EditTicket ref="editTicket" :item="item" @editItem="editItem"></EditTicket>
                <DeleteTicket :item="item" @getTickets="getTickets"></DeleteTicket>
            </template>
        </v-data-table>
    </div>
</template>

<script>
import EditTicket from './actions/EditTicket.vue';
import DeleteTicket from './actions/DeleteTicket.vue';
export default {
    name: 'Table',
    components: {
    EditTicket,
    DeleteTicket
},
    data () {
        return {
            dialogEdit: false,
            dialogDelete: false,
            editedIndex: null,
            editedItem: [],
            headers: [
                {
                    text: 'ID',
                    align: 'center',
                    justify: 'center',
                    sortable: true,
                    value: 'id',
                },
                { text: 'Título', value: 'title' },
                { text: 'Empresa', value: 'corporate_name' },
                { text: 'Solicitante', value: 'customer_name' },              
                { text: 'Opções', value: 'actions', sortable: false },
            ],
            tickets: [],
            error: '',
        }
    }, 
    created () {
        this.getTickets()
    },
    methods: {
        updateDialog (dialog) {
            this.dialog = dialog
        },
        async getTickets () {
            let date = new URLSearchParams(window.location.search).get('date')
            let worker = new URLSearchParams(window.location.search).get('worker')
            let status = new URLSearchParams(window.location.search).get('status')
            const req = await fetch(process.env.HOST_BACK+'/getTicket/?'+new URLSearchParams({
                company: localStorage.getItem('company'),
                token: localStorage.getItem('refresh'),
                date: date,
                worker: worker,
                status: status,
            }),{
                method: 'GET',
            })
            const res = await req.json()
            this.tickets = res
        },
        editItem (item) {
            this.editedIndex = this.tickets.indexOf(item)
            this.editedItem = Object.assign({}, item)
            this.$refs.editTicket.getCustomers(this.editedItem.corporate_id)
        },

        deleteItem (item) {
            this.editedIndex = this.tickets[this.tickets.indexOf(item)].id
            this.editedItem = Object.assign({}, item)
            this.dialogDelete = true
        },
    },
}
</script>

<style scoped>
#main-suport-table {
    grid-area: table;

    overflow-y: scroll;
    padding: 1vw;
}
</style>