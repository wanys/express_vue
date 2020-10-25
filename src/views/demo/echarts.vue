<template>
  <div class="mod-demo-echarts">
    <el-alert
      title="提示："
      type="warning"
      :closable="false">
      <div slot-scope="description">
        <p class="el-alert__description">1. 此Demo只提供ECharts官方使用文档，入门部署和体验功能。具体使用请参考：http://echarts.baidu.com/index.html</p>
      </div>
    </el-alert>

    <el-row :gutter="20">
      <el-col :span="12">
        <el-card>
          <div id="J_chartLineBox" class="chart-box"></div>
        </el-card>
      </el-col>
      <el-col :span="12">
        <el-card>
          <div id="J_chartBarBox" class="chart-box"></div>
        </el-card>
      </el-col>
      <el-col :span="12">
        <el-card>
          <div id="J_chartPieBox" class="chart-box"></div>
        </el-card>
      </el-col>
      <el-col :span="12">
        <el-card>
          <div id="J_chartScatterBox" class="chart-box"></div>
        </el-card>
      </el-col>
    </el-row>
  </div>
</template>

<script>
  import echarts from 'echarts'
  import App from '../../App'
export default {
    data () {
      return {
        chartLine: null,
        chartBar: null,
        chartPie: null,
        chartScatter: null
      }
    },
    mounted () {
      this.initChartLine()
      this.initChartBar()
      this.initChartPie()
      this.initChartScatter()
      this.getCharLineData()
      this.getCharBarData()
      this.getCharPieData()
    },
    activated () {
      // 由于给echart添加了resize事件, 在组件激活时需要重新resize绘画一次, 否则出现空白bug
      if (this.chartLine) {
        this.chartLine.resize()
      }
      if (this.chartBar) {
        this.chartBar.resize()
      }
      if (this.chartPie) {
        this.chartPie.resize()
      }
      if (this.chartScatter) {
        this.chartScatter.resize()
      }
    },
    methods: {
      // 折线图
      initChartLine () {
        var option = {
          'title': {
            'text': '月份订单的变化'
          },
          'tooltip': {
            'trigger': 'axis'
          },
          // 'legend': {
          //   'data': [ '揽件', '派件' ]
          // },
          'grid': {
            'left': '3%',
            'right': '4%',
            'bottom': '3%',
            'containLabel': true
          },
          'toolbox': {
            'feature': {
              'saveAsImage': { }
            }
          },
          'xAxis': {
            'type': 'category',
            'boundaryGap': false,
            axisLabel: {
              interval: 0 // 横轴信息全部显示
            }
            // 'data': [ '周一', '周二', '周三', '周四', '周五', '周六', '周日' ]
          },
          'yAxis': {
            'type': 'value'
          },
          'series': [
            // {
            //   'name': '揽件',
            //   'type': 'line'
            //   // 'stack': '总量'
            //   // 'data': [ 120, 132, 101, 134, 90, 230, 210 ]
            // },
            {
              'name': '订单',
              'type': 'line',
              label: {
                normal: {
                  show: true
                  // 柱上字体样式
                  // textStyle: {
                  //   fontSize: 12,
                  //   fontWeight: 'bolder'
                  // }
                }
              }
              // 'stack': '总量'  stack多条数据堆积到一个柱体
            }
          ]
        }
        this.chartLine = echarts.init(document.getElementById('J_chartLineBox'))
        this.chartLine.setOption(option)
        window.addEventListener('resize', () => {
          this.chartLine.resize()
        })
      },
      getCharLineData () {
        this.$http({
          url: this.$http.adornUrl('/express/echars/line'),
          method: 'get',
          params: this.$http.adornParams()
        }).then(({data}) => {
          if (data && data.code === 0) {
            console.log(data.taskLine)
            this.chartLine.setOption({
              'xAxis': {
                'data': data.taskLine.month
              },
              'series': [
                // {
                //   'name': '揽件',
                //   'data': data.taskLine.collect
                // },
                {
                  'name': '订单',
                  'data': data.taskLine.orderCount
                }
              ]
            })
          }
        })
      },
      // 柱状图
      initChartBar () {
        var option = {
          'title': {
            'text': '各月份派件和揽件任务'
          },
          tooltip: {
            trigger: 'axis',
            axisPointer: {
              type: 'shadow'
            }
          },
          legend: {
            x: '200px',
            y: '5px',
            data: ['揽件', '派件']
          },
          grid: {
            left: '3%',
            right: '4%',
            bottom: '3%',
            containLabel: true
          },
          xAxis: [
            {
              type: 'category',
              axisLabel: {
                interval: 0 // 横轴信息全部显示
                // rotate: -15 // -15度角倾斜显示
              }
              // X轴月份竖直显示
              // axisLabel: {
              //   interval: 0,
              //   formatter: function (value) {
              //     return value.split('').join('\n')
              //   }
              // }
            }
          ],
          yAxis: [
            {
              type: 'value'
            }
          ],
          series: [
            {
              name: '揽件',
              type: 'bar',
              label: {
                normal: {
                  show: true
                  // 柱上字体样式
                  // textStyle: {
                  //   fontSize: 12,
                  //   fontWeight: 'bolder'
                  // }
                }
              }
              // data: [320, 332, 301, 334, 390, 330, 320]
            },
            {
              name: '派件',
              type: 'bar',
              label: {
                normal: {
                  show: true,
                  textStyle: {
                    fontSize: 12,
                    fontWeight: 'bolder'
                  }
                }
              }
              // stack: '广告'
            }
          ]
        }
        this.chartBar = echarts.init(document.getElementById('J_chartBarBox'))
        this.chartBar.setOption(option)
        window.addEventListener('resize', () => {
          this.chartBar.resize()
        })
      },
      getCharBarData () {
        this.$http({
          url: this.$http.adornUrl('/express/echars/bar'),
          method: 'get',
          params: this.$http.adornParams()
        }).then(({data}) => {
          if (data && data.code === 0) {
            this.chartBar.setOption({
              xAxis: [
                {
                  data: data.taskBar.month
                }
              ],
              series: [
                {
                  name: '揽件',
                  // type: 'bar',
                  data: data.taskBar.collect
                },
                {
                  name: '派件',
                  // type: 'bar',
                  // stack: '广告',
                  data: data.taskBar.send
                }
              ]
            })
          }
        })
      },
      // 饼状图
      initChartPie () {
        var option = {
          title: {
            text: '各等级评分占比',
            subtext: '本季度',
            left: 'center'
          },
          tooltip: {
            trigger: 'item',
            formatter: '{a} <br/>{b} : {c} ({d}%)'
          },
          legend: {
            orient: 'vertical',
            left: 'left',
            data: ['非常满意', '满意', '一般', '不满意', '非常不满意']
          },
          series: [
            {
              name: '占比',
              type: 'pie',
              radius: '55%',
              center: ['50%', '60%'],
              data: [
                {value: 0.354, name: '非常满意'},
                {value: 0.396, name: '满意'},
                {value: 0.198, name: '一般'},
                {value: 0.039, name: '不满意'},
                {value: 0.026, name: '非常不满意'}
              ],
              emphasis: {
                itemStyle: {
                  shadowBlur: 10,
                  shadowOffsetX: 0,
                  shadowColor: 'rgba(0, 0, 0, 0.5)'
                }
              }
            }
          ]
        }
        this.chartPie = echarts.init(document.getElementById('J_chartPieBox'))
        this.chartPie.setOption(option)
        window.addEventListener('resize', () => {
          this.chartPie.resize()
        })
      },
      getCharPieData () {
        this.$http({
          url: this.$http.adornUrl('/express/echars/pie'),
          method: 'get',
          params: this.$http.adornParams()
        }).then(({data}) => {
          if (data && data.code === 0) {
            console.log(data)
            console.log(data.taskPie.month)
            console.log(data.taskPie.collect)
            console.log(data.taskPie.send)
            this.chartPie.setOption({
              series: [
                {
                  name: '访问来源',
                  data: [
                    { value: 535, name: '揽件' },
                    { value: 310, name: '派件' }
                  ].sort(function (a, b) { return a.value - b.value })
                }
              ]
            })
          }
        })
      },
      // 散点图
      initChartScatter () {
        var option = {
          title: {
            text: '各方向表现优劣'
          },
          tooltip: {},
          /* legend: {
            data: ['服务方向']
          }, */
          radar: {
            // shape: 'circle',
            name: {
              textStyle: {
                color: '#fff',
                backgroundColor: '#999',
                borderRadius: 3,
                padding: [3, 5]
              }
            },
            indicator: [
              {name: '服务方向', max: 0.5},
              {name: '速度方向', max: 0.5},
              {name: '安全方向', max: 0.5}
            ]
          },
          series: [{
            name: '综合',
            type: 'radar',
            radius: '55%',
            center: ['50%', '80%'],
            // areaStyle: {normal: {}},
            data: [
              {
                value: [0.354, 0.396, 0.354],
                name: '服务方向'
              }
            ]
          }]
        }
        this.chartScatter = echarts.init(document.getElementById('J_chartScatterBox'))
        this.chartScatter.setOption(option)
        window.addEventListener('resize', () => {
          this.chartScatter.resize()
        })
      }
    }
  }
</script>

<style lang="scss">
  .mod-demo-echarts {
    > .el-alert {
      margin-bottom: 10px;
    }
    > .el-row {
      margin-top: -10px;
      margin-bottom: -10px;
      .el-col {
        padding-top: 10px;
        padding-bottom: 10px;
      }
    }
    .chart-box {
      min-height: 400px;
    }
  }
</style>
