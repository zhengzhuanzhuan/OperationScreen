<template>
    <chart :options="polar" auto-resize></chart>
</template>
<style lang="scss" scoped>
.echarts {
  width: 100%;
  height: 100%;
}
</style>
<script>
import echarts from 'echarts'
export default {
  name: 'lineChart',
  props: ['draw_coupon_count', 'used_coupon_count', 'dt_date'],
  watch: {
    draw_coupon_count: {
      handler: function (val, oldval) {
        // console.log(val)
        this.polar.series[0].data = val
      },
      deep: true
    },
    used_coupon_count: {
      handler: function (val, oldval) {
        this.polar.series[1].data = val
      },
      deep: true
    },
    dt_date: {
      handler: function (val, oldval) {
        this.polar.xAxis[0].data = val
      },
      deep: true
    }
  },
  data () {
    return {
      polar: {
        legend: {
          right: 10,
          width: 500,
          top: 0,
          itemWidth: 10,
          itemHeight: 10,
          textStyle: {
            fontSize: 12,
            color: 'rgba(255,255,255,0.4)'
          },
          data: ['发放数', '使用数']
        },
        grid: {
          left: '3%',
          right: '4%',
          top: '20%',
          bottom: '10%',
          containLabel: true
        },
        xAxis: [
          {
            type: 'category',
            data: [],
            axisLine: {
              lineStyle: {
                color: '#2A3675'
              }
            },
            axisTick: {
              show: false
            },
            axisLabel: {
              margin: 10,
              textStyle: {
                fontSize: 12,
                color: 'rgba(255,255,255,0.4)'
              }
            }
          }
        ],
        yAxis: [
          {
            type: 'value',
            axisLine: {
              show: false
            },
            axisLabel: {
              margin: 10,
              textStyle: {
                fontSize: 14,
                color: 'rgba(255,255,255,0.4)'
              }
            },
            splitLine: {
              show: true,
              interval: 'auto',
              lineStyle: {
                color: ['#2A3675']
              }
            }
          }
        ],
        series: [
          {
            name: '发放数',
            type: 'bar',
            barWidth: '20%',
            itemStyle: {
              normal: {
                color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [
                  { offset: 0, color: '#33e6ef' },
                  { offset: 1, color: '#2e9dff' }
                ])
              }
            },
            data: []
          },
          {
            name: '使用数',
            type: 'bar',
            barWidth: '20%',
            itemStyle: {
              normal: {
                color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [
                  { offset: 0, color: '#fa9696' },
                  { offset: 1, color: '#ff007c' }
                ])
              }
            },
            data: []
          }
        ]
      }
    }
  }
}
</script>