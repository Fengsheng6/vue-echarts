<template>
    <div ref="chart" style="width: 100%; height: 400px;"></div>
  </template>
   
  <script>
  import * as echarts from 'echarts';

  export default {
    name: 'LineChart',
    props: {
      options: {
        type: Object,
        required: true
      }
    },
    mounted() {
      this.createChart();
    },
    watch: {
      options: {
        deep: true,
        handler() {
          this.createChart();
        }
      }
    },
    methods: {
      createChart() {
        const chart = echarts.init(this.$refs.chart);
        chart.setOption(this.options);
      }
    }
  };
  </script>
  
  <style scoped>
  </style>
  


