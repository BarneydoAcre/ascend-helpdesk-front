<template>
    <div>
        <h1>Dashboard {{ $route.params.company}}</h1>
        
    </div>
</template>

<script>
    export default {
        name: 'dashboard',
        data () {
            return {
                fab: false
            }
        },
        methods: {
        },  
    }
</script>