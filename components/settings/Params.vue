<template>
    <div class="main-settings-params">
        <v-card width="25vw">
            <v-card-title>Módulo Suporte</v-card-title>
            <v-card-text>
                <v-list dense nav>
                    <AddTicketStatus></AddTicketStatus>
                    <AddRequestType></AddRequestType>
                </v-list>
            </v-card-text>
        </v-card>
    </div>
</template>

<script>
import AddTicketStatus from './actions/AddTicketStatus.vue';
import AddRequestType from './actions/AddRequestType.vue';
export default {
    name: "Params",
    components: {
        AddTicketStatus ,
        AddRequestType
    }
}
</script>

<style scoped>
.main-settings-params {
    grid-area: params;
}
.card-suport {
    border: 1px solid #d8d8d8;
    border-radius: 1vh;
    padding: 1vh;
    width: 40%;
}
</style>