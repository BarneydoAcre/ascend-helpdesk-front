<template>
    <div class="main-settings">
        <Params></Params>
    </div>
</template>

<script>
import Params from '../../components/settings/Params.vue';
export default {
    name: "Settings",
    components: {
        Params,
    },
}
</script>

<style>
#main-settings {
    display: grid;
    grid-template-columns: 20% 20% 20% 20% 20%;
    grid-template-rows: 19.2vh 19.2vh 19.2vh 19.2vh 19.2vh;
    grid-template-areas: 'params params params params params'
                         'params params params params params'
                         'table table table table table'
                         'table table table table table'
                         'table table table table table';
}
</style>