<template>
  <div ref="chart" style="width: 100%; height: 400px;"></div>
</template>

<script>
import * as echarts from 'echarts';

export default {
  name: 'EChartsComponent',
  props: {
    options: {
      type: object,
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
  },
  data() {
    return {
      chartOptions: {
        title: {
          text: 'ECharts 示例'
        },
        tooltip: {
          trigger: 'axis'
        },
        xAxis: {
          type: 'category',
          data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun']
        },
        yAxis: {
          type: 'value'
        },
        series: [
          {
            name: '销售量',
            type: 'bar',//柱形图bar,折线图line,散点图scatter
            data: [120, 200, 150, 80, 70, 110, 130]
          }
        ]
      }
    };
  }
};
</script>

<style scoped>
</style>
