<template>
    <v-dialog v-model="dialog" max-width="85vw">
        <template v-slot:activator="{ on, attrs }">
            <v-btn 
            fab
            small
            color="primary" 
            v-bind="attrs" 
            v-on="on"
            >
                <v-icon>mdi-hamburger-plus</v-icon>
            </v-btn>
        </template>
        <v-card>
        </v-card>
    </v-dialog>
  
</template>

<script>
export default {
    name: "AddProduct",
    data () {
        return {
            dialog: false,
        }
    }
}
</script>
