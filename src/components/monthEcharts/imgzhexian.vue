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
            bottom: 0,
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
              axisTick: {
                //y轴刻度线
                show: false
              }
            }
          ],
          series: [
            {
              name: this.chartsOption.legendName[0],
              type: "line",
              barWidth: "20%",
              symbol:"circle",
              symbolSize:8,
              itemStyle: {
                normal: {
                  label: {
                    show: true,
                    position: "top",
                    color:"#000"
                  },
                  lineStyle: {
                    color: "#4BACC6"
                  },
                  color: "#F79646"
                }
              },
              data: this.value.xValues
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
  top: 40px;
  left: 50%;
  font-weight: bold;
  color: #000;
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