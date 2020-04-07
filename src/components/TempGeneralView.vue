<template>
  <div id="temperaturePie" ref="charts" style="width:800px;height:640px"></div>
</template>

<script>
var echarts = require("echarts");
export default {
  data() {
    return {
      option: {

        title: {
          text: "小区体温情况统计",
          left: "center",
          top: 10,
          textStyle: {
            color: "#2c343c",
            fontSize:30
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
            radius: ['20%', '70%'],
            center: ["50%", "50%"],
            data: [
              { value: 100, name: "体温正常" ,itemStyle:{color:"#67C23A"}},
              { value: 5, name: "体温偏高" ,itemStyle:{color:"#F56C6C"}},
              { value: 20, name: "体温偏低" ,itemStyle:{color:"#409EFF"}}
            ].sort(function(a, b) {
              return a.value - b.value;
            }),
            roseType: "radius",
            label:{
                fontSize:15,
                fontWeight:"bold"
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
    },
  },
  mounted(){
      this.$nextTick(() => {
        this.drawLine();
      });
  }
  
};
</script>