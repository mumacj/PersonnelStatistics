<template>
  <div>
    <div id="temperaturePie" ref="charts" style="width:700px;height:640px;display:inline-block"></div>
    <div id="temperatureLine" ref="charts2" style="width:800px;height:640px;display:inline-block"></div>
  </div>
</template>

<script>
var echarts = require("echarts");
export default {
  data() {
    return {
      option2: {
        title: {
          show: true,
          text: "当日进入小区人员体温散点图",
          x: "right",
          y: "top",
          textAlign: "center",
          textStyle: {
            fontSize: 25,
            fontFamily: "Courier New"
          }
        },
        xAxis: {
          type: "category",
          name: "日期",
          nameTextStyle: {
            fontWeight: 600,
            fontSize: 18
          }
        },
        yAxis: {
          type: "value",
          name: "体温/℃",
          nameTextStyle: {
            fontWeight: 600,
            fontSize: 18
          },
          min: "35.5",
          max: "38.5",
          
        },
        visualMap: {
            show: false,
          type: "piecewise",
          pieces: [
            { min: 37.3,max:38.5 , color:"#F56C6C" }, 
            { min: 36.0, max: 37.2, color:"#67C23A" },
            { min: 35.5, max: 35.9, color:"#409EFF" }
          ]
        },
        tooltip: { trigger: "axis" },
        series: [
          {
            symbol: "circle",
            symbolSize: 10,
            name: "",
            data: [
              ["2020/4/9 8:00", 35.5],
              ["2020/4/9 8:01", 36.9],
              ["2020/4/9 8:02", 37.5],
              ["2020/4/9 8:03", 38.1],
              ["2020/4/9 8:04", 36.3],
              ["2020/4/9 8:05", 36.9],
              ["2020/4/9 8:06", 37.24],
              ["2020/4/9 8:07", 36.26],
              ["2020/4/9 8:08", 36.84],
              ["2020/4/9 8:09", 37.22],
              ["2020/4/9 8:10", 36.68],
              ["2020/4/9 9:10", 36.6],
              ["2020/4/9 10:10", 36.8],
              ["2020/4/9 12:10", 35.68],
              ["2020/4/9 13:10", 37.68],
              ["2020/4/9 4:10", 36.3],
              ["2020/4/9 3:10", 36.2],
              ["2020/4/9 2:10", 36.5],
              ["2020/4/9 19:10", 36.4],
              ["2020/4/9 20:10", 36.7],
              ["2020/4/9 22:10", 36.9]
            ],
            type: "scatter",
            itemStyle: {
              normal: {
                color: "#F56C6C",
                lineStyle: {
                  color: "#40C6FF",
                  width: 5
                }
              }
            }
          }
        ]
      },
      option: {
        title: {
          text: "小区体温情况统计",
          left: "center",
          top: 10,
          textStyle: {
            color: "#2c343c",
            fontSize: 30
          }
        },

        tooltip: {
          trigger: "item",
          formatter: "{a} <br/>{b} : {c} ({d}%)"
        },

        series: [
          {
            name: "体温状况",
            type: "pie",
            hoverAnimation: true,
            radius: ["20%", "70%"],
            center: ["50%", "50%"],
            data: [
              { value: 100, name: "体温正常", itemStyle: { color: "#67C23A" } },
              { value: 5, name: "体温偏高", itemStyle: { color: "#F56C6C" } },
              { value: 20, name: "体温偏低", itemStyle: { color: "#409EFF" } }
            ].sort(function(a, b) {
              return a.value - b.value;
            }),
            roseType: "radius",
            label: {
              fontSize: 15,
              fontWeight: "bold"
            },
            labelLine: {
              smooth: 0.2,
              length: 30,
              length2: 20
            },

            animationType: "scale",
            animationEasing: "bounceOut",
            animationDelay: function(idx) {
              return Math.random() * 200;
            }
          }
        ]
      }
    };
  },
  methods: {
    drawLine() {
      let myChart = echarts.init(this.$refs.charts);
      myChart.setOption(this.option);
      let myChart2 = echarts.init(this.$refs.charts2);
      myChart2.setOption(this.option2);
    }
  },
  mounted() {
    this.$nextTick(() => {
      this.drawLine();
    });
  }
};
</script>