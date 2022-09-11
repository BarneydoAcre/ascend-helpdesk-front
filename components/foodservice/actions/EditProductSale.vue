<template>
    <v-dialog v-model="dialog" max-width="85vw">
        <template v-slot:activator="{ on, attrs }">
            <v-card 
            v-bind="attrs" 
            v-on="on">
                <v-card-title>
                    {{ product.name }}
                </v-card-title>
                <v-card-text class="d-flex justify-space-between align-center">
                    <div>
                        {{ product.cost }}
                    </div>
                    <div>
                        {{ product.brand }}
                    </div>
                </v-card-text>
            </v-card>
        </template>
        <v-card>
            <v-card-title>
                Editar Produto
            </v-card-title>
            <v-card-text>
                
            </v-card-text>
        </v-card>
    </v-dialog>
  
</template>

<script>
export default {
    name: "EditProductSale",
    props: ["product",],
    data () {
        return {
            dialog: false,
        }
    },
    methods: {
    }
}
</script>
