<template>
<div class ="world-map">
    <div id="world-map-off" style="display:none"></div>
    <div id="world_map" style="display:flex"></div>
</div>
<button id="change" onclick="map_control()">
    <span id="on" style="display:flex;justify-content:center;align-items:center">主场</span>
    <span id="off" style="display:none;justify-content:center;align-items:center">客场</span>
</button>
</template>
   
<script>
import {defineComponent, toRaw} from "vue";
import axios from "axios";
import world from "./world.json"
 
export default defineComponent({
    name: "WorldMap",
  data() {
    return {
      num: [],
    }
  },
 
  methods: {
 
    competition() {
      // 获取需要的数据
      let zhuchang_data = []
      let kechang_data = []
 
      axios.get("http://127.0.0.1:5000/competition").then(res => {
        // 获取数据
        zhuchang_data = toRaw(res.data.zhuchang_data)
        kechang_data = toRaw(res.data.kechang_data)
        console.log(zhuchang_data)
        console.log(kechang_data)
 
        //  生成出线方法
        function formtGCData(geoData, data, srcNam, dest) {
          var tGeoDt = []
          if (dest) {
            for (var i = 0; i < data.length; i++) {
              if (srcNam !== data[i].name) {
                tGeoDt.push({
                  coords: [geoData[srcNam], geoData[data[i].name]]
                })
              }
            }
          } else {
            for (var j = 0; j < data.length; j++) {
              if (srcNam !== data[j].name) {
                tGeoDt.push({
                  coords: [geoData[data[j].name], geoData[srcNam]]
                })
              }
            }
          }
          return tGeoDt
        }
 
        //  生成进线方法
        function formtVData(geoData, data, srcNam) {
          var tGeoDt = []
          for (var i = 0; i < data.length; i++) {
            var tNam = data[i].name
            if (srcNam !== tNam) {
              tGeoDt.push({
                name: tNam,
                value: geoData[tNam]
              })
            }
          }
          tGeoDt.push({
            name: srcNam,
            value: geoData[srcNam],
            symbolSize: 8,
          })
          tGeoDt.forEach((item) => {
            if (item.name === 'china') {
              item.itemStyle = {
                color: 'red'
              }
 
            } else {
              item.itemStyle = {
                color: '#9E992F'
              }
 
            }
          })
          return tGeoDt
        }
 
        // 显示点的坐标
        let geoCoordMap = {
          china: [117.3, 41.03],
          Botswana: [24.68, -22.33],
          Canada: [-110.895168, 60.2312],
          Brazil: [-50.895168, -10.2312],
        };
        // 所在点对应数据,上面数据添加修改后务必这里添加修改，value可不用
        let data = [
          {
            id: 1,
            name: 'china',
 
          }, {
            id: 2,
            name: 'Botswana',
          }, {
            id: 3,
            name: 'Canada',
          }, {
            id: 4,
            name: 'Brazil',
          }]
        //箭头类型 //''circle', 'rect', 'roundRect', 'triangle', 'diamond', 'pin',  'arrow', 'none''
        var planePath = 'arrow'
        // 定义图表
        this.$echarts.registerMap('world', world)
        let world_map = this.$echarts.init(document.getElementById("world_map"), "football_customed");
        // 定义主场图表的配置
        let option = {
          geo: {
            type: "map",
            map: 'world',
            zoom: 1.2,//地图的缩放
            label: {
              emphasis: {
                show: true,
                color: '#fff'
              }
            },
            roam: true,//是否滚动缩放
            regions: [
              {
                name: 'China', //这个名字可以是map（世界地图），China（中国地图高亮）
                itemStyle: {
                  normal: {
                    areaColor: '#480103',
                    borderColor: '#ffc107',
                    borderWidth: 1,
                  },
                  emphasis: {
                    areaColor: '#b40106',
                  },
                },
              },
            ],
            itemStyle: {
              normal: {
                areaColor: '#690613',
                borderColor: 'rgba(255,255,255,0.6)',//国界线颜色
                borderType: 'dotted',//国界线类型
              },
              emphasis: {
                areaColor: '#ad9541'
              }
            },
          },
          series: [
            {
              type: 'lines',//飞线
              zlevel: 1,
              effect: {
                show: true,
                period: 6,
                trailLength: 0.1,
                color: '#fff',//箭头颜色
                symbol: planePath,
                symbolSize: 8,
              },
              lineStyle: {
                normal: {
                  color: '#278FA2',//线的颜色
                  width: 1,
                  opacity: 0.4,
                  curveness: 0.2,
                  type: 'dotted',//'dotted'点型虚线 'solid'实线 'dashed'线性虚线
                },
                emphasis: {
                  type: 'dotted',
                  color: 'yellow',//线的颜色
                }
 
              },
              data: formtGCData(geoCoordMap, data, 'china', false)
            },
            {
              type: 'scatter',//圆点
              coordinateSystem: 'geo',
              zlevel: 10,
              symbolSize: 10,
              hoverAnimation: false,
              silent: true,
              data: formtVData(geoCoordMap, data, 'Brazil')
            },
          ]
        }
        // 配置图表
        world_map.setOption(option)
        // 图表响应式改变
        window.addEventListener('resize', function () {
          world_map.resize();
        });
      })
 
    },
    competition_off() {
      // 获取需要的数据
      let zhuchang_data = []
      let kechang_data = []
 
      axios.get("http://127.0.0.1:5000/competition").then(res => {
        // 获取数据
        zhuchang_data = toRaw(res.data.zhuchang_data)
        kechang_data = toRaw(res.data.kechang_data)
 
        //  生成出线方法
        function formtGCData(geoData, data, srcNam, dest) {
          var tGeoDt = []
          if (dest) {
            for (var i = 0; i < data.length; i++) {
              if (srcNam !== data[i].name) {
                tGeoDt.push({
                  coords: [geoData[srcNam], geoData[data[i].name]]
                })
              }
            }
          } else {
            for (var j = 0; j < data.length; j++) {
              if (srcNam !== data[j].name) {
                tGeoDt.push({
                  coords: [geoData[data[j].name], geoData[srcNam]]
                })
              }
            }
          }
          return tGeoDt
        }
 
        //  生成进线方法
        function formtVData(geoData, data, srcNam) {
          var tGeoDt = []
          for (var i = 0; i < data.length; i++) {
            var tNam = data[i].name
            if (srcNam !== tNam) {
              tGeoDt.push({
                name: tNam,
                value: geoData[tNam]
              })
            }
          }
          tGeoDt.push({
            name: srcNam,
            value: geoData[srcNam],
            symbolSize: 8,
          })
          tGeoDt.forEach((item) => {
            if (item.name === 'china') {
              item.itemStyle = {
                color: 'red'
              }
 
            } else {
              item.itemStyle = {
                color: '#9E992F'
              }
 
            }
          })
          return tGeoDt
        }
 
        // 显示点的坐标
        let geoCoordMap = {
          china: [117.3, 41.03],
          Botswana: [24.68, -22.33],
          Canada: [-110.895168, 60.2312],
          Brazil: [-50.895168, -10.2312],
        };
        // 所在点对应数据,上面数据添加修改后务必这里添加修改，value可不用
        let data = [
          {
            id: 1,
            name: 'china',
 
          }, {
            id: 2,
            name: 'Botswana',
          }, {
            id: 3,
            name: 'Canada',
          }, {
            id: 4,
            name: 'Brazil',
          }]
        //箭头类型 //''circle', 'rect', 'roundRect', 'triangle', 'diamond', 'pin',  'arrow', 'none''
        var planePath = 'arrow'
        // 定义图表
        this.$echarts.registerMap('world', world)
        let map_off = this.$echarts.init(document.getElementById("world_map_off"), "football_customed");
        // 定义主场图表的配置
        let option = {
          geo: {
            type: "map",
            map: 'world',
            zoom: 1.2,//地图的缩放
            label: {
              emphasis: {
                show: true,
                color: '#fff'
              }
            },
            roam: true,//是否滚动缩放
            regions: [
              {
                name: 'China', //这个名字可以是map（世界地图），China（中国地图高亮）
                itemStyle: {
                  normal: {
                    areaColor: '#480103',
                    borderColor: '#ffc107',
                    borderWidth: 1,
                  },
                  emphasis: {
                    areaColor: '#b40106',
                  },
                },
              },
            ],
            itemStyle: {
              normal: {
                areaColor: '#690613',
                borderColor: 'rgba(255,255,255,0.6)',//国界线颜色
                borderType: 'dotted',//国界线类型
              },
              emphasis: {
                areaColor: '#ad9541'
              }
            },
          },
          series: [
            {
              type: 'lines',//飞线
              zlevel: 1,
              effect: {
                show: true,
                period: 6,
                trailLength: 0.1,
                color: '#fff',//箭头颜色
                symbol: planePath,
                symbolSize: 8,
              },
              lineStyle: {
                normal: {
                  color: '#278FA2',//线的颜色
                  width: 1,
                  opacity: 0.4,
                  curveness: 0.2,
                  type: 'dotted',//'dotted'点型虚线 'solid'实线 'dashed'线性虚线
                },
                emphasis: {
                  type: 'dotted',
                  color: 'yellow',//线的颜色
                }
 
              },
              data: formtGCData(geoCoordMap, data, 'china', false)
            },
            {
              type: 'scatter',//圆点
              coordinateSystem: 'geo',
              zlevel: 10,
              symbolSize: 10,
              hoverAnimation: false,
              silent: true,
              data: formtVData(geoCoordMap, data, 'Brazil')
            },
          ]
        }
        // 配置图表
        map_off.setOption(option)
        // 图表响应式改变
        window.addEventListener('resize', function () {
          map_off.resize();
        });
      })
 
    },
 
 
  },
 
  mounted() {
    this.competition()
    this.competition_off()
  },
})
 
</script>