<template>
    <v-dialog v-model="dialog" max-width="40vw">
        <template v-slot:activator="{ on, attrs }">
            <v-btn 
            color="primary"
            width="10vw" 
            v-bind="attrs" 
            v-on="on">
                <v-icon>mdi-filter</v-icon> Filtros
            </v-btn>
        </template>
        <v-card>
            <v-card-title>
                <h4>Filtros Aplicados</h4>
            </v-card-title>
            <v-card-text>
                <v-form v-model="valid" ref="form" lazy-validation>
                    <v-row dense>
                        <v-col cols="12">
                            <v-menu ref="menu"
                            v-model="menu"
                            :close-on-content-click="false"
                            transition="scale-transition"
                            offset-y
                            min-width="auto">
                                <template v-slot:activator="{ on, attrs }">
                                    <v-text-field
                                    dense
                                    filled
                                    readonly
                                    label="Período"
                                    v-model="form.date"
                                    v-bind="attrs"
                                    v-on="on"
                                    ></v-text-field>
                                </template>
                                <v-date-picker
                                    v-model="form.date"
                                    :active-picker.sync="activePicker"
                                    :max="(new Date(Date.now() - (new Date()).getTimezoneOffset() * 60000)).toISOString().substr(0, 10)"
                                    min="1950-01-01"
                                    range
                                    @change="save"
                                ></v-date-picker>
                            </v-menu>
                        </v-col>
                        <v-col cols="12">
                            <v-select
                            dense
                            filled
                            label="Operador"
                            v-model="form.worker"
                            :items="workers" 
                            item-text="worker_name" 
                            item-value="worker_id"></v-select>
                        </v-col>
                        <v-col cols="12">
                            <v-select
                            dense
                            filled
                            label="Status"
                            v-model="form.status" 
                            :items="status" 
                            item-text="status_name" 
                            item-value="status_id"></v-select>
                        </v-col>
                        <v-col cols="12" class="d-flex justify-center">
                            <v-btn 
                            color="primary"
                            @click="applyFilter(form)">
                                <v-icon>mdi-filter</v-icon>
                                Aplicar
                            </v-btn>
                            <v-btn 
                            color="warning"
                            class="ml-4"
                            @click="clearFilter">
                                <v-icon>mdi-clear</v-icon>
                                Limpar Filtros
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
    name: "TicketFilter",
    emits: ['getTickets'],
    data () {
        return {
            dialog: false,
            valid: false,
            menu: false,
            activePicker: null,
            form: {
                date: null,
                worker: null,
                status: null,
            },
            status: [],
            workers: [],
        }
    },
    mounted () {
        this.getTicketStatus()
        this.getCompanyWorkers()
    },
    methods: {
        save (date) {
            this.$refs.menu.save(date)
        },
        applyFilter (form) {
            history.pushState({}, null, '?'+new URLSearchParams({
                date: form.date,
                worker: form.worker,
                status: form.status,
            }))
            this.dialog = false
            this.$emit('getTickets')
        },
        clearFilter () {
            history.pushState({}, null, '?'+new URLSearchParams({
                date: 'null',
                worker: 'null',
                status: 'null',
            }))
            this.dialog = false
            this.$emit('getTickets')

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
        async getCompanyWorkers () {
            const req = await fetch(process.env.HOST_BACK+'/getCompanyWorkers/?'+ new URLSearchParams({
                company: localStorage.getItem('company'),
                token: localStorage.getItem('refresh')
            }),{
                method: 'GET',
            })
            const res = await req.json()
            this.workers = res
        },
    }
}
</script>