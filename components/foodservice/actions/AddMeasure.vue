<template>
    <v-dialog v-model="dialog" max-width="55vw">
        <template v-slot:activator="{ on, attrs }">
            <v-btn 
            v-bind="attrs" 
            v-on="on"
            height="70%">
                <v-icon>mdi-plus</v-icon>
            </v-btn>
        </template>
        <v-card>
            <v-card-title>
                Nova Unidade de Medida
            </v-card-title>
            <v-card-text>
                <v-form v-model="valid" ref="form" lazy-validation>
                    <v-row dense>
                        <v-col cols="10">
                            <v-text-field dense filled label="Sigla da Und. de Medida" counter="2" v-model="form.measure" :rules="rules"></v-text-field>
                        </v-col>
                        <v-col cols="2">
                            <v-btn 
                            fab
                            color="green"
                            height="70%"
                            @click="addMeasure">
                                <v-icon>mdi-send</v-icon>
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
    name: "Measure",
    emits: ["getMeasure",],
    data () {
        return {
            dialog: false,
            valid: false,
            form: {
                company: localStorage.getItem("company"),
                company_worker: localStorage.getItem("user_id"),
                measure: null,
            },
            rules: [
                v => !!v || "Obrigatório",
                v => (v && v.length == 2) || "Deve conter 2 caracteres"
            ],
        }
    },
    methods: {
        validate () {
            this.$refs.form.validate()
        },
        addMeasure () {
            this.validate();
            setTimeout(async () => {
                if (this.valid != false) {
                    const req = await fetch(process.env.HOST_BACK + "/foodservice/addMeasure/", {
                        method: "POST",
                        body: JSON.stringify(this.form),
                        headers: { "Content-Type": "application/json" },
                    });
                    if (req.status == 200) {
                        this.dialog = false;
                        this.$emit("getMeasure");
                    }
                    else {
                    }
                }
            }, 0);
        },
    }
    
}
</script>