<template>
  <div>
    <div ref="lineChart" style="width: 1000px; height: 400px; margin-top: 2200px;"></div>
    <div ref="barChart" style="width: 1000px; height: 400px; margin-top: 20px;"></div>
    <div ref="pieChart" style="width: 1000px; height: 400px; margin-top: 20px;"></div>
    <div ref="scatterChart" style="width: 1000px; height: 400px; margin-top: 20px;"></div>
    <div ref="chinaMap" style="width: 1000px; height: 600px; margin-top: 20px;"></div>
    
  </div>
</template>

<script>
import * as echarts from 'echarts';
import axios from 'axios';


export default {
  name: 'EChartsComponent',
  props: {
    lineOptions: Object,
    barOptions: Object,
    pieOptions: Object,
    scatterOptions: Object,
    chinaMapOptions: Object,
  },
  mounted() {
    this.initCharts();
    this.loadChinaMap();
  },
  watch: {
    lineOptions: 'updateCharts',
    barOptions: 'updateCharts',
    pieOptions: 'updateCharts',
    scatterOptions: 'updateCharts',
    chinaMapOptions: 'updateCharts',
  },
  methods: {
    initCharts() {
      this.lineChart = echarts.init(this.$refs.lineChart);
      this.barChart = echarts.init(this.$refs.barChart);
      this.pieChart = echarts.init(this.$refs.pieChart);
      this.scatterChart = echarts.init(this.$refs.scatterChart);
      this.chinaMapChart = echarts.init(this.$refs.chinaMap);
    },
    loadChinaMap() {
      axios.get('https://geo.datav.aliyun.com/areas_v3/bound/100000_full.json')
        .then(response => {
          echarts.registerMap('china', response.data);
          this.updateCharts();
        })
        .catch(error => {
          console.error('Failed to load China map', error);
        });
    },
    updateCharts() {
      this.lineChart && this.lineChart.setOption(this.lineOptions);
      this.barChart && this.barChart.setOption(this.barOptions);
      this.pieChart && this.pieChart.setOption(this.pieOptions);
      this.scatterChart && this.scatterChart.setOption(this.scatterOptions);
      this.chinaMapChart && this.chinaMapChart.setOption(this.chinaMapOptions);
    },
  }
}
</script>

<style scoped>
</style>
