<template>
    <v-dialog v-model="dialog">
        <template v-slot:activator="{ on, attrs }">
            <v-btn
            fab
            small
            color="primary" 
            v-bind="attrs" 
            v-on="on">
                <v-icon>mdi-printer</v-icon>
            </v-btn>
        </template>
    </v-dialog>
</template>

<script>
export default {
    name: "Print",
    data () {
        return {
            dialog: false,
        }
    }
}
</script>