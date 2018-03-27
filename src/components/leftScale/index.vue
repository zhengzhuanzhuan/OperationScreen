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
export default {
  props: ['scale'],
  name: 'lineChart',
  data () {
    return {
      polar: {
        grid: {
          y: -10,
          left: '7%',
          right: '2%'
        },
        series: [
          {
            // name: '访问来源',
            type: 'pie',
            hoverAnimation: false,
            startAngle: '60',
            radius: ['55%', '95%'],
            itemStyle: {
              normal: {
                label: {
                  show: true,
                  color: '#fff',
                  fontSize: '20',
                  position: 'outside'
                },
                labelLine: {
                  show: true,
                  length: 10,
                  length2: 10,
                  lineStyle: {
                    color: '#fff',
                    width: 2
                  }
                }
              }
            },
            data: [
              {
                value: 0,
                name: '',
                itemStyle: {
                  normal: {
                    // 边框大小和边框颜色
                    borderWidth: '10',
                    borderColor: '#0e1f71',
                    color: {
                      colorStops: [
                        {
                          offset: 0,
                          color: '#fa9696'
                        },
                        {
                          offset: 0.3,
                          color: '#fa9696'
                        },
                        {
                          offset: 1,
                          color: '#ff007c'
                        }
                      ]
                    }
                  }
                }
              },
              {
                value: 0,
                name: '',
                itemStyle: {
                  normal: {
                    color: {
                      colorStops: [
                        {
                          offset: 0,
                          color: '#00bbff'
                        },
                        {
                          offset: 0.6,
                          color: '#00ffff'
                        }
                      ]
                    }
                  }
                }
              }
            ]
          }
        ]
      }
    }
  },
  watch: {
    scale: {
      handler: function (val, oldval) {
        this.polar.series[0].data[0].name =
          '使用券' + val[0].with_coupon_rate || 0
        this.polar.series[0].data[0].value = (val[0].with_coupon_rate).slice(0, 2)
        this.polar.series[0].data[1].name =
          '未用券' + val[0].without_coupon_rate || 0
        this.polar.series[0].data[1].value = (val[0].without_coupon_rate).slice(0, 2)
      },
      deep: true
    }
  }
}
</script>