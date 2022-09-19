<template>
    <v-dialog v-model="dialog" max-width="50vw">
        <template v-slot:activator="{ on, attrs }">
            <v-icon @click="dialog == true" v-bind="attrs" v-on="on" small class="mr-2">mdi-delete</v-icon>
        </template>
        <v-card>
            <v-card-title class="d-flex justify-center">
                <h3>Deseja realmente remover este ticket?</h3>
            </v-card-title>
            <v-card-title dense class="d-flex justify-center">
                <h6 style="color: red;">{{ error }}</h6>
            </v-card-title>
            <v-card-text>
                <v-container>
                    <v-row dense>
                        <v-col align="center" cols="12" md="12" no-gutter>
                            <v-btn color="error" class="mr-4" @click="deleteTicket(item)">
                                <v-icon>mdi-delete</v-icon>Remover</v-btn>
                            <v-btn color="warning" class="mr-4" @click="dialog = false">Sair</v-btn>
                        </v-col>
                    </v-row>
                </v-container>
            </v-card-text>
        </v-card>
    </v-dialog>
</template>

<script>
export default {
    name: 'DeleteProductSale',
    props: ['item'],
    
    data () {
        return {
            dialog: false,
        }
    },
    methods: {
        async deleteItem (item) {
            this.$emits.deleteItem()
        }
    }
}
</script>