<template lang="pug">
  .chartMain
    div.zhuzhuangtu(:class="this.chartsOption.className")
    p {{ chartsOption.tableName }}
    //- p.titleLeft 配变台区数(台)
</template>
<script>
import echarts from "echarts";
export default {
  props: {
    value: {
      type: Object,
      default: () => {
        return {
          xData: [
            "10月",
            "11月",
            "12月",
            "13月",
            "14月",
            "15月",
            "16月"
          ],
          serverData: [
            [102, 52, 200, 334, 90, 100, 220]
          ]
        };
      }
    },
    chartsOption: {
      type: Object,
      default: () => {
        return {
          className: "", //类名
          isRow: true, // x轴标签横向纵向显示，true为横向，false为纵向
          legendName: ["北京地区10kv线路同期线损率(%)"], //图例显示文字
          tableName: "图1 北京地区同期线损率周趋势" //表格名称
        };
      }
    }
  },
  data() {
    return {};
  },
  created() {},
  mounted() {
    this.initChart();
  },
  methods: {
    initChart() {
      this.setChart().then(
        option => {
          let myChart = echarts.init(
            document.querySelector("." + this.chartsOption.className)
          );
          myChart.setOption(option);

          window.addEventListener("resize", () => {
            myChart.resize();
          });
        },
        error => {
          console.log(error);
        }
      );
    },
    setChart() {
      return new Promise((resolve, reject) => {
        let option = {
          tooltip: {
            trigger: "axis",
            axisPointer: {
              // 坐标轴指示器，坐标轴触发有效
              type: "shadow" // 默认为直线，可选为：'line' | 'shadow'
            }
          },
          grid: {
            top: "17%",
            left: "3%",
            right: "4%",
            bottom: "17%",
            containLabel: true
          },
          legend: {
            show: true,
            top: 0,
            data:this.chartsOption.legendName
          },
          xAxis: [
            {
              type: "category",
              data: this.value.xNames,
              axisTick: {
                show:false,
                alignWithLabel: false
              },
              axisLine: {
                show: false
              },
              axisLabel: {
                interval: 0,
                formatter: value => {
                  return this.chartsOption.isRow
                    ? value
                    : value.split("").join("\n");
                }
              }
            }
          ],
          yAxis: [
            {
              type: "value",
              axisLine: {
                //y轴
                show: false
              },
              axisLabel:{
                show: false
              },
              axisTick: {
                //y轴刻度线
                show: false
              }
            }
          ],
          series: [
            {
              name: this.chartsOption.legendName[0],
              type: "bar",
              barWidth: "20%",
              barGap:0,//柱间距离
              itemStyle: {
                normal: {
                  label: {
                    show: true,
                    position: [-5,-15],
                    color:"#15B450"
                  },
                  color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [{
                    offset: 0,
                    color: '#15B450'
                  }, {
                      offset: 0.8,
                      color: '#BFDD98'
                  }], false)
                }
              },
              data: this.value.xValues1
            },
            {
              name: this.chartsOption.legendName[1],
              type: "bar",
              barWidth: "20%",
              itemStyle: {
                normal: {
                  label: {
                    show: true,
                    position: [5,-15],
                    color:"#0079C7"
                  },
                  color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [{
                    offset: 0,
                    color: '#0079C7'
                  }, {
                      offset: 0.8,
                      color: '#48BBEB'
                  }], false)
                }
              },
              data: this.value.xValues2
            }
          ]
        };
        resolve(option);
      }).catch(err => {
        reject(err);
      });
    }
  }
};
</script>
<style lang='less' scoped>
.chartMain {
  width: 100%;
  height: 300px;
  // flex: 1;
  // padding-bottom: 20px;
  // border: 1px solid #ccc;
  box-sizing: border-box;
  position: relative;
  padding-top: 40px;
}
.zhuzhuangtu {
  width: 100%;
  height: 99%;
  // border: 1px solid #ccc;
  margin: 0 auto;
}
p {
  text-align: center;
  position: absolute;
  bottom: -25px;
  left: 50%;
  transform: translate(-50%);
}
.titleLeft {
  height: 20px;
  color: #000;
  position: absolute;
  left: -2%;
  top: 10%;
  transform: rotate(-90deg) translate(-50%, 0%);
}
</style>