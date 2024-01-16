<template>
  <div>
    <div>【云端报警风险】</div>
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
    // 雷达图的坐标系配置
    radar: {
      // 坐标轴外部支点名字的样式
      name: {
        textStyle: {
          color: '#05D5FF',
          fontSize: 14
        },
      },
      shape: 'polygon', //六边形
      center: ['50%', '50%'],
      radius: '80%', //半径
      startAngle: 120, //倾斜角度
      // 中轴线
      axisLine: {
        lineStyle: {
          color: 'rgba(5,213,255,.8)',
        }
      },
      // 网格线
      splitLine: {
        show: true,
        lineStyle: {
          width: 1,
          color: 'rgba(5,213,255,.8)'
        }
      },
      // 指示器
      indicator: props.data.risks.map((item) => {
        return {
          name: item.name,
          max: 100
        }
      }),
      splitArea: {
        show: false,
      }
    },
    // 坐标极点
    polar: {
      center: ['50%', '50%'],
      radius: '0%',
    },
    // 坐标角度
    angleAxis: {
      min: 0,
      interval: 5,
      clockwise: false,
    },
    // 径向轴
    radiusAxis: {
      min: 0,
      interval: 20,
      splitLine: {
        show: true
      }
    },
    // 核心配置
    series: {
      type: 'radar',
      symbol: 'circle',
      symbolSize: 10,
      itemStyle: {    //点
        normal: {
          color: '#05D5FF'
        }
      },
      areaStyle: {
        normal: {
          color: '#05D5FF',
          opacity: 0.5
        }
      },
      lineStyle: {
        width: 2,
        color: '#05D5FF',
      },
      label: {
        normal: {
          show: true,
          color: '#05D5FF'
        }
      },
      data: [{
        value: props.data.risks.map((item => {
          return item.value
        }))
      }]
    }
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
