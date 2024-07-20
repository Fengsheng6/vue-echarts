<template>
  <div id="app">
    <EChartsComponent 
      :lineOptions="lineChartOptions" 
      :barOptions="barChartOptions" 
      :pieOptions="pieChartOptions" 
      :scatterOptions="scatterChartOptions"
      :chinaMapOptions="chinaMapOptions"
      :worldMapOptions="worldMapOptions"
    />
    <div id="main" style="width: 1000px; height: 600px; margin-top: 20px;"></div>
  </div>
  
</template>

<script>
import EChartsComponent from './components/EChartsComponent.vue';
import * as echarts from 'echarts';
import worldJson from './components/map/world.json';

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
        legend: {
          data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'],
          right: "10%",
          top: "30%",
          orient: "vertical"
        },
        series: [
          {
            type: "pie",
            label: {
              show: true,
              formatter: "{b} : {c} ({d}%)"
            },
            data: [
              { value: 463, name: "Mon" },
              { value: 395, name: "Tue" },
              { value: 157, name: "Wed" },
              { value: 149, name: "Thu" },
              { value: 147, name: "Fri" },
              { value: 250, name: "Sat" },
              { value: 114, name: "Sun" }
            ]
          }
        ]
      },
      scatterChartOptions: {
        title: {
          text: 'Scatter Chart'
        },
        tooltip: {
          trigger: 'axis'
        },
        legend: {
          data: ['scatter']
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
            name: 'scatter',
            type: 'scatter',
            data: [120, 132, 101, 134, 90, 230, 210]
          }
        ]
      },
      chinaMapOptions: {
        title: {
          text: 'China Map'
        },
        tooltip: {
          trigger: 'item'
        },
        visualMap: {
          min: 0,
          max: 100000000,
          left: 'left',
          top: 'bottom',
          text: ['High', 'Low'],
          calculable: true
        },
        series: [
          {
            name: 'Population',
            type: 'map',
            map: 'china',
            roam: true,
            itemStyle: {
              emphasis: { label: { show: true } }
            },
            data: [
              { name: '北京市', value: 21540000 },
              { name: '上海市', value: 24240000 },
              { name: '广东省', value: 140000000 },
              { name: '浙江省', value: 120000000 },
              { name: '四川省', value: 100000000 }
            ]
          }
        ]
      },
      worldMapOptions: {
        title: {
          text: 'World Map'
        },
        tooltip: {
          trigger: 'item'
        },
        visualMap: {
          min: 0,
          max: 100000000,
          left: 'left',
          top: 'bottom',
          text: ['High', 'Low'],
          calculable: true
        },
        series: [
          {
            name: 'Population',
            type: 'map',
            map: 'world',
            roam: true,
            itemStyle: {
              emphasis: { label: { show: true } }
            },
            data: [
              { name: 'China', value: 1393000000 },
              { name: 'United States', value: 328200000 },
              { name: 'Japan', value: 126400000 }
            ]
          }
        ]
      }
    };
  },
  mounted() {
    this.worldCharts();
  },
  methods: {
    worldCharts() {
      const chartDom = document.getElementById('main');
      const myChart = echarts.init(chartDom);
      echarts.registerMap('world', worldJson);
      const option = {
        tooltip: {
          trigger: 'item',
        },
        visualMap: {
          min: 0,
          max: 100000,
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
              { name: 'China', value: 139536.11 },
              { name:'United States', value: 32820 },
              //more items...

            ]
          }
        ]
      };
      myChart.setOption(option);
    },
  }
}
</script>

<style>
html, body, #app {
  margin: 0;
  padding: 0;
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}

</style>
