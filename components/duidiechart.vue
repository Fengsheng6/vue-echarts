<template>
    <div id="main" class="echart-style">
    </div>
  </template>
   
  <script setup lang="ts">
  import * as echarts from 'echarts';
  import { onMounted, ref } from 'vue';
  let myChart = ref()
  let option = ref({})
  // let data = ref<any>([
  //   [15, 0],
  //   [-50, 10],
  //   [-56.5, 20],
  //   [-46.5, 30],
  //   [-22.1, 40]
  // ])
  onMounted(() => {
    init()
  })
  const init = () => {
    // 基于准备好的dom，初始化echarts实例
    myChart.value = echarts.init(document.getElementById('main'));
    option.value = {
    xAxis: {
      data: ['A', 'B', 'C', 'D', 'E']
    },
    yAxis: {},
    series: [
      {
        data: [10, 22, 28, 43, 49],
        type: 'bar',
        stack: 'x'
      },
      {
        data: [5, 4, 3, 5, 10],
        type: 'bar',
        stack: 'x'
      }
    ]
  };
    myChart.value.setOption(option.value)
  };
  </script>
  <style scoped>
  .echart-style {
    width: 1000px;
    height: 900px;
    background: skyblue;
  }
  </style>