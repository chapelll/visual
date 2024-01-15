<template>
  <div>
    <div>【服务资源占用比】</div>
    <div ref="target" class="w-full h-full"></div>
  </div>
</template>

<script setup>

import { ref, onMounted, watch } from 'vue'
import * as echarts from 'echarts'
const props = defineProps({
  data: {
    type: Object,
    required: true,
  }
})
let myChart = null
let target = ref(null)
onMounted(() => {
  myChart = echarts.init(target.value)
  renderChart()
})

const renderChart = () => {
  let options = {
    xAxis: {
      type: 'category',
      data: props.data.servers.map((item) => {
        return item.name
      }),
      axisLabel: {
        color: '#9EB1C8'
      },
      axisTick: { //展示x轴上的刻度
        show: false,
      },
    },
    yAxis: {
      type: 'value',
      show: false,
      max: function (value) {
        return parseInt(value.max * 1.2)
      }
    },
    grid: {
      top: 64,
      right: 32,
      bottom: 56,
      left: 32,
      containLabel: true, //计算时包含标签
    },
    series: [
      {
        type: 'bar', //柱形图
        data: props.data.servers.map((item) => {
          return {
            name: item.name,
            value: item.value,
          }
        }),
        itemStyle: {  //每个柱子的样式
          color: '#479AD3',
          barBorderRadius: [5, 5, 0, 0], //轴的圆角
          shadowColor: 'rgba(0,0,0,0.3)',
          shadowBlur: 5, //阴影模糊
        },
        barWidth: 12,
        label: { //每个柱子的名字
          show: true,
          position: 'top',
          textStyle: {
            color: '#fff'
          },
          formatter: function (value, index) {
            return `${value.data.value}%`
          }
        },
      }
    ],
  }
  myChart.setOption(options)
}

watch(() => props.data, () => {
  renderChart()
})
</script>

<style scoped lang="scss"></style>
