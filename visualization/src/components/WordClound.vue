<template>
  <div>
    <div>【文档云图】</div>
    <div ref="target" class="w-full h-full"></div>
  </div>
</template>

<script setup>
import { defineProps, onMounted, ref, watch } from 'vue'
import * as echarts from 'echarts'
import 'echarts-wordcloud'

const props = defineProps({
  data: {
    type: Object,
    required: true
  }
})

// 1.初始化echarts对象
let myChart = null
const target = ref(null)

const randomColor = () => {
  const r = Math.floor(Math.random() * 255)
  const g = Math.floor(Math.random() * 255)
  const b = Math.floor(Math.random() * 255)
  return `rgb(${r},${g},${b})`
}

onMounted(() => {
  myChart = echarts.init(target.value)
  renderChart() //渲染方法
})
// 2.配置options
const renderChart = () => {
  const options = {
    series: [
      {
        type: 'wordCloud',
        sizeRange: [8, 46], //字体大小范围
        rotationRange: [0, 0], //倾斜范围
        gridSize: 0, //字的间隔
        layoutAnimation: true, //渲染动画
        textStyle: {
          color: randomColor //随机赋予颜色
        },
        emphasis: {  //高亮
          textStyle: {
            color: '#FFF',
            fontSize: 'bold'
          }
        },
        data: props.data.datas
      }
    ]
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
