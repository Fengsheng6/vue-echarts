<template>
  <div id="app">
    <EChartsComponent :lineOptions="lineChartOptions" :barOptions="barChartOptions" :pieOptions="pieChartOptions" />
    <WorldMap/>
    <button @click="updateChartData">更新数据</button>
  </div>
  
</template>

<script>
import { ref } from 'vue'
import EChartsComponent from './components/EChartsComponent.vue';
import WorldMap from './components/map/index.vue'
import * as echarts from 'echarts'
import worldMap from './components/map/world.json'

export default {
  name: 'App',
  components: {
    EChartsComponent
  },
  data() {
    return {
      lineChartOptions: {
        title: {
          text: 'Line Chart'
        },
        tooltip: {
          trigger: 'axis'
        },
        legend: {
          data: ['Line']
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
            name: 'Line',
            type: 'line',
            data: [120, 132, 101, 134, 90, 230, 210]
          }
        ]
      },
      barChartOptions: {
        title: {
          text: 'Bar Chart'
        },
        tooltip: {
          trigger: 'axis'
        },
        legend: {
          data: ['Bar']
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
            name: 'Bar',
            type: 'bar',
            data: [220, 182, 191, 234, 290, 330, 310]
          }
        ]
      },
      pieChartOptions: {
        title: {
          text: 'Pie Chart'
        },
        tooltip: {
          trigger: 'axis'
        },
        legend: {
        // 图例
          data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'],
          right: "10%",
          top: "30%",
          orient: "vertical"
        },
        series: [
          {
            type: "pie", // type设置为饼图
            label: {
                show: true,
                formatter: "{b} : {c} ({d}%)" // b代表名称，c代表对应值，d代表百分比
            },
            data: [
              {
                value: 463,
                name: "Mon"
              },
              {
                value: 395,
                name: "Tue"
              },
              {
                value: 157,
                name: "Wed"
              },
              {
                value: 149,
                name: "Thu"
              },
              {
                value: 147,
                name: "Fri"
              },
              {
                value: 250,
                name: "Sat"
              },
              {
                value: 114,
                name: "Sun"
              },
            ]
          }
        ]
      },

    };
  },
  methods: {
    updateData() {
      // 更新折线图数据
      this.lineChartOptions.series[0].data = [150, 132, 110, 140, 100, 220, 230];
      
      // 更新柱状图数据
      this.barChartOptions.series[0].data = [210, 190, 180, 240, 300, 340, 320];
    }
  },
  
};
</script>

<style>
#app {
  width: 100%;
  height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
button {
  margin-top: 20px;
  padding: 10px 20px;
  font-size: 16px;
}
</style>



var chartDom = document.getElementById('main');
      var myChart = echarts.init(chartDom);
      var option;

      echarts.registerMap('world', worldJson);

      option = {
        tooltip: {
          trigger: 'item',
        },
        visualMap: {
          min: 0,
          max: 10000,
          text: ['High', 'Low'],
          realtime: false,
          calculable: true,
          inRange: {
            color: ['lightskyblue', 'yellow', 'orangered']
          }
        },
        series: [
          {
            name: 'World Population',
            type: 'map',
            map: 'world',
            roam: true,
            itemStyle: {
              emphasis: { label: { show: true } }
            },
            data: [
              { name: 'Afghanistan', value: 28397.812 },
              { name: 'Angola', value: 19549.124 },
              { name: 'Albania', value: 3150.448 },
              { name: 'United Arab Emirates', value: 4975.593 },
              { name: 'Argentina', value: 4037.282 },
              { name: 'Armenia', value: 2963.493 },
              { name: 'French Southern and Antarctic Lands', value: 36.746 },
              { name:'China',value:13953.611},
              // 更多数据
            ]
          }
        ]
      };
      myChart.setOption(option);