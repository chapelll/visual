<template>
  <div>
    <!-- <div>【地图可视化】</div> -->
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
  console.log(props.data);
  myChart = echarts.init(target.value)
  renderChart()
})

const renderChart = () => {
  const options = {
    // 时间轴
    timeline: {
      data: props.data.voltageLevel,
      axisType: 'category',
      autoPlay: true, //自动切换时间轴
      playInterval: 3000, //切换间隔时间
      left: '10%',
      right: '10%',
      bottom: '0%',
      width: '80%',
      label: {
        normal: {
          textStyle: {
            color: '#DDD'
          }
        },
        emphasis: {
          textStyle: {
            color: '#FFF'
          }
        },
      },
      symbolSize: 10,
      lineStyle: {
        color: '#555'
      },
      checkpointStyle: { //选中点的样式
        borderColor: '#888',
        borderWidth: 2
      },
      controlStyle: { //控件的样式，时间轴左右两边的箭头
        showNextBtn: true,
        showPrevBtn: true,
        normal: {
          color: '#666',
          borderColor: '#666'
        },
        emphasis: {
          color: '#aaa',
          borderColor: '#aaa'
        },
      }
    },
    // 柱形图
    baseOption: {
      grid: {          //设置方位
        right: '2%',
        top: '15%',
        bottom: '10%',
        width: '20%'
      },
    },
    options: []

  }

  // 展示多个柱形图
  props.data.voltageLevel.forEach((item, index) => {
    options.options.push({
      backgroundColor: '#142037',
      title: [{
        text: '2019 ~ 2023 年度数据统计',
        left: '0%',
        top: '0%',
        textStyle: {
          color: '#ccc',
          fontSize: 30
        }
      }, {
        id: 'statistic',  //副标题
        text: item + '年数据统计情况',
        right: '0%',
        top: '4%',
        textStyle: {
          color: '#ccc',
          fontSize: 20
        }
      }],
      xAxis: {
        type: 'value',
        scale: true, //可以不出现0刻度
        position: 'top',
        splitLine: {
          show: false,
        },
        axisLine: {
          show: false, //不显示轴线
        },
        axisTick: {
          show: false, //不显示刻度
        },
        axisLabel: {
          margin: 2,
          textStyle: {
            color: '#aaa'
          }
        },
      },
      yAxis: {
        type: 'category',
        axisLine: {
          show: true,
          lineStyle: {
            color: '#ddd',
          },
        },
        axisTick: {
          show: false, //不显示刻度
        },
        axisLabel: {
          interval: 0,
          textStyle: {
            color: '#ddd'
          }
        },
        data: props.data.categoryData[item].map((item) => item.name)
      },
      series: [{
        type: 'bar',
        zlevel: 1.5,
        itemStyle: {
          normal: {
            color: props.data.colors[index],
          }
        },
        data: props.data.categoryData[item].map((item) => {
          return item.value
        })
      }]
    })
  })


  myChart.setOption(options)
}

watch(() => props.data, () => {
  renderChart()
})
</script>

<style scoped lang="scss"></style>
