<template>
  <div>
    <div>【数据传递信息】</div>
    <div ref="target" class="w-full h-full"></div>
  </div>
</template>

<script setup>
import { defineProps, onMounted, ref, watch } from 'vue'
import * as echarts from 'echarts'

const props = defineProps({
  data: {
    type: Object,
    required: true
  }
})

// 1.初始化echarts对象
let myChart = null
const target = ref(null)
onMounted(() => {
  myChart = echarts.init(target.value)
  renderChart() //渲染方法
})
// 2.配置options
const renderChart = () => {
  const options = {
    xAxis: {
      show: false,
      type: 'value'
    },
    yAxis: {
      show: false,
      type: 'value'
    },
    series: [
      {
        type: 'graph', //关系图
        layout: 'none', //不需要布局类型
        coordinateSystem: 'cartesian2d', //坐标系选择二位直角坐标系
        symbolSize: 26, //节点大小
        z: 3,
        edgeLabel: { //边界的标签文字
          normal: {
            show: true,
            color: '#fff',
            textStyle: {
              fontSize: 14
            },
            formatter: function (params) {
              return params.data.speed
            }
          }
        },
        label: { //圆饼下方的文字
          normal: {
            show: true,
            position: 'bottom',
            color: '#5E5E5E',
          }
        },
        edgeSymbol: ['none', 'arrow'],  //边两边的标签类型
        edgeSymbolSize: 8,
        data: props.data.relations.map((item) => { //处理数据(一大四小)
          if (item.id !== 0) {
            // 非数据中心
            return {
              name: item.name,
              category: 0,
              active: true, //活动的数据,
              speed: `${item.speed}kb/s`,
              value: item.value
            }
          } else {
            // 数据中心
            return {
              name: item.name,
              value: item.value,
              symbolSize: 100, //数据中心较大，单独指定size
              itemStyle: {
                normal: {
                  color: { //渐变色
                    colorStops: [
                      {
                        offset: 0,
                        color: '#157EFF'
                      }, {
                        offset: 1,
                        color: '#35C2FF'
                      },]
                  }
                }
              },
              label: {
                normal: {
                  fontSize: 14
                }
              }
            }
          }
        }),
      }
    ],
  }
  // 3.echarts对象实例化渲染
  myChart.setOption(options)
}

//数据一变化就渲染
watch(() => props.data, () => {
  renderChart()
})
</script>

<style scoped lang="scss"></style>
