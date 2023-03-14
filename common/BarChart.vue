<template>
    <div>
        <highcharts :options="chartOptions" ref="lineCharts"></highcharts>
    </div>
</template>

<script>
/* eslint-disable */

import { Chart } from "highcharts-vue";
import Highcharts from "highcharts";
import stockInit from "highcharts/modules/stock";

stockInit(Highcharts);


export default {
    props: ['chartData','question'],

    components: {
        highcharts: Chart
    },
    data() {
        return {
            res:[],
            chartOptions: {
                chart: {
                    type: 'bar',
                    marginLeft: 150
                },
                title: {
                    text: this.question
                },
                // subtitle: {
                //     text: 'Source: <a href="https://highcharts.uservoice.com/forums/55896-highcharts-javascript-api">UserVoice</a>'
                // },
                xAxis: {
                    type: 'category',
                    title: {
                        text: null
                    },
                    min: 0,
                    max: 3,
                    scrollbar: {
                        enabled: false
                    },
                    tickLength: 0
                },
                yAxis: {
                    min: 0,
                    max: 100,
                    title: {
                        text: 'Votes(In %)',
                        align: 'high'
                    }
                },
                plotOptions: {
                    bar: {
                        dataLabels: {
                            enabled: true
                        }
                    }
                },
                legend: {
                    enabled: false
                },
                credits: {
                    enabled: false
                },
                series: [{
                    name: 'Votes',
                    data: this.chartData
                }]
            }
        };
    },
    mounted() {
        console.log(this.chartData)
      
    }
};
</script>
