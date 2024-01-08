<template>
  <div>
    <div>【大区数据信息】</div>
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
    // X轴展示数据
    xAxis: {
      show: false,
      type: 'value',
      max: function (value) {
        return parseInt(value.max) * 1.2
      }
    },
    // Y轴展示数据
    yAxis: {
      type: 'category',
      data: props.data.regions.map((item) => {
        return item.name
      }),
      inverse: true,
      axisLine: { //不展示轴线
        show: false,
      },
      axisTick: { //不展示刻度
        show: false,
      },
      axisLabel: {
        color: '#9eb1c8'
      },
    },
    // 图标绘制位置
    grid: {
      top: 0,
      right: 0,
      bottom: 0,
      left: 0,
      containLabel: true, //计算时包含标签
    },
    //核心配置
    series: [{
      type: 'bar', //柱形图
      data: props.data.regions.map((item) => {
        return {
          name: item.name,
          value: item.value,
        }
      }),
      showBackground: true,
      backgroundStyle: {
        color: 'rgba(180,180,180,0.2)'
      },
      itemStyle: {  //每个柱子的样式
        color: '#479AD3',
        barBorderRadius: 5, //轴的圆角
        shadowColor: 'rgba(0,0,0,0.3)',
        shadowBlur: 5, //阴影模糊
      },
      barWidth: 12,
      label: { //每个柱子的名字
        show: true,
        position: 'right',
        textStyle: {
          color: '#fff'
        }
      },
    }],
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
