<template>
    <div id="main-suport">
        <Action @getTickets="$refs.table.getTickets()"></Action>
        <Table ref="table"></Table>
        <Graphs></Graphs>
    </div>
</template>

<script>
import Table from '../../components/suport/Table.vue'
import Action from '../../components/suport/Action.vue'
import Graphs from '../../components/suport/Graphs.vue'

export default {
    name: 'suport',
    components: {
        Table,
        Action,
        Graphs,
    },
    data () {
        return {
        }
    },
    methods: {
    },
    created () {
        this.$vuetify.theme.dark = false
    },
}
</script>

<style>
#main-suport {
    display: grid;
    grid-template-columns: 20% 20% 20% 20% 20%;
    grid-template-rows: 19.2vh 19.2vh 19.2vh 19.2vh 19.2vh;
    grid-template-areas: 'action action graphs graphs graphs'
                         'none none none none none'
                         'table table table table table'
                         'table table table table table'
                         'table table table table table';
}
</style>