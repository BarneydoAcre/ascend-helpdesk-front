<template>
    <div id="main-suport-graphs">
        <div :style="{ height: '30vh' }">
            <div id="fluxo_20_dias_chart"></div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Graphs',
    head() {
        return {
            script: [
                {src: 'https://cdn.jsdelivr.net/npm/chart.js'},
                {src: "https://code.highcharts.com/highcharts.js",body: true},
                {src: "https://code.highcharts.com/modules/accessibility.js",body: true},
            ],
        }
    },
    methods: {
        async fluxoDeCaixa () {
            const req = await fetch(this.hostBack+'/fluxo_de_caixa')
            const res = await req.json()

            const total_vendas_mensal_chart = Highcharts.chart('fluxo_20_dias_chart', {
                plotOptions: {
            //         series: {
            //             // general options for all series
            //         },
            //         spline: {
            //         },
                },
            //     chart: {
            //         height: parseInt(window.screen.height)*0.32
            //     },
            //     title: {
            //         text: undefined,
            //     },
            //     xAxis: {
            //     }
            })
        }
    }
}
</script>

<style scoped>
#main-suport-company {
    grid-area: graphs;

    /* background: grey; */
    padding: 1vh;
}
</style>