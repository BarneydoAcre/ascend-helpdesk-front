<template>
    <div id="main-suport-action" class="d-flex flex-wrap justify-space-around">
        <AddCustomerCompany></AddCustomerCompany>
        <AddCustomerCompanyWorker></AddCustomerCompanyWorker>
        <AddTicket @getTickets="$emit('getTickets')"></AddTicket>
        <TicketFilter @getTickets="$emit('getTickets')"></TicketFilter>
    </div>
</template>

<script>
import AddTicket from "./actions/AddTicket.vue";
import AddCustomerCompany from "./actions/AddCustomerCompany.vue";
import AddCustomerCompanyWorker from "./actions/AddCustomerCompanyWorker.vue";
import TicketFilter from "./actions/TicketFilter.vue";
export default {
    name: "Action",
    emits: ['getTickets'],
    components: {
    AddTicket,
    AddCustomerCompany,
    AddCustomerCompanyWorker,
    TicketFilter
},
}
</script>

<style scoped>
#main-suport-action {
    grid-area: action;
}
</style>