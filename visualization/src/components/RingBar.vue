<template>
  <div>
    <div>【大区异常处理】</div>
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

const getSeriesData = () => {
  const series = []

  props.data.abnormals.forEach((item, index) => {
    // 上层
    series.push({
      name: item.name,
      type: 'pie',
      clockWise: false, //逆时针
      hoverAnimation: false, //hover无效果
      radius: [73 - 15 * index + '%', 68 - 15 * index + '%'], //每层环的半径差距是15%
      center: ['55%', '55%'],
      label: {
        show: false
      },
      data: [{
        value: item.value,
        name: item.name,
      }, {
        value: 1000,
        itemStyle: {
          color: 'rgba(0,0,0,0)',
          borderWidth: 0,
        },
        tooltip: {
          show: false
        },
        hoverAnimation: false
      }]
    })
    // 下层
    series.push({
      name: item.name,
      type: 'pie',
      clockWise: false, //逆时针
      hoverAnimation: false, //hover无效果
      radius: [73 - 15 * index + '%', 68 - 15 * index + '%'], //每层环的半径差距是15%
      center: ['55%', '55%'],
      label: {
        show: false
      },
      silent: true,
      z: 1,
      data: [{
        value: 7.5,
        itemStyle: {
          color: 'rgb(3,31,62)',
          borderWidth: 0,
        },
        tooltip: {
          show: false
        },
        hoverAnimation: false
      }, {
        value: 2.5,
        itemStyle: {
          color: 'rgba(0,0,0,0)',
          borderWidth: 0,
        },
      },]
    })
  })


  return series
}

// 2.配置options
const renderChart = () => {
  const options = {
    // 图例配置
    legend: {
      show: true,
      icon: 'circle',
      top: '14%',
      left: '60%',
      data: props.data.abnormals.map((item) => {
        return item.name
      }),
      width: -5, //竖向排列
      itemWidth: 10,
      itemHeight: 10,
      itemGap: 5,
      textStyle: {
        fontSize: 12,
        lineHeight: 5,
        color: '#fff',
      }
    },
    // 提示层
    tooltip: {
      show: true,
      trigger: 'item', //触发器
      formatter: '{a}<br>{b}:{c}({d}%)',
    },
    // Y轴
    yAxis: [{
      type: 'category',
      inverse: true,
      axisLine: {
        show: false //Y轴线隐藏
      }
    }],
    // X轴
    xAxis: [{
      show: false
    }],
    // 核心配置
    series: getSeriesData()
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
