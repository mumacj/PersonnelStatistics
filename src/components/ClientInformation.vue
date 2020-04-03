<template>
  <div>
    <el-table size="small" :data="tableData" height="570" stripe style="width: 100%">
      <el-table-column prop="date" label="注册日期" width="180" align="center"></el-table-column>
      <el-table-column prop="name" label="姓名" width="180" align="center"></el-table-column>
      <el-table-column prop="address" label="住户地址" align="center"></el-table-column>
      <el-table-column prop="inTimes" label="出入小区/次" align="center"></el-table-column>
      <el-table-column label="操作" align="center">
        <template slot-scope="scope">
          <el-button size="mini" type="primary" @click="handleCheck(scope.$index, scope.row)">查看</el-button>
          <el-button size="mini" type="warning" @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
        </template>
      </el-table-column>
    </el-table>
    <el-pagination
      :page-sizes="[10, 20, 30, 40]"
      :page-size="10"
      layout="total, sizes, prev, pager, next, jumper"
      :total="400"
    ></el-pagination>
    <el-dialog :visible.sync="dialogTableVisible" @open="open()">
      <div id="temperatureCharts" ref="charts" style="width:900px;height:500px"></div>
    </el-dialog>
  </div>
</template>

<script>
var echarts = require("echarts");
export default {
  data() {
    return {
      dialogTableVisible: false,
      tableData: [
        {
          date: "2016-05-02",
          name: "王小虎",
          address: "上海市普陀区金沙江路 1518 弄",
          inTimes:6
        },
        {
          date: "2016-05-04",
          name: "王小虎",
          address: "上海市普陀区金沙江路 1517 弄",
          inTimes:6
        },
        {
          date: "2016-05-01",
          name: "王小虎",
          address: "上海市普陀区金沙江路 1519 弄",
          inTimes:6
        },
        {
          date: "2016-05-03",
          name: "王小虎",
          address: "上海市普陀区金沙江路 1516 弄",
          inTimes:6
        },
        {
          date: "2016-05-03",
          name: "王小虎",
          address: "上海市普陀区金沙江路 1516 弄",
          inTimes:6
        },
        {
          date: "2016-05-03",
          name: "王小虎",
          address: "上海市普陀区金沙江路 1516 弄",
          inTimes:6
        },
        {
          date: "2016-05-03",
          name: "王小虎",
          address: "上海市普陀区金沙江路 1516 弄",
          inTimes:5
        },
        {
          date: "2016-05-03",
          name: "王小虎",
          address: "上海市普陀区金沙江路 1516 弄",
          inTimes:5
        },
        {
          date: "2016-05-03",
          name: "王小虎",
          address: "上海市普陀区金沙江路 1516 弄",
          inTimes:5
        },
        {
          date: "2016-05-03",
          name: "王小虎",
          address: "上海市普陀区金沙江路 1516 弄",
          inTimes:5
        },
        {
          date: "2016-05-03",
          name: "王小虎",
          address: "上海市普陀区金沙江路 1516 弄",
          inTimes:5
        }
      ],
      option: {
        title: {
          show: true,
          text: "",
          x: "center",
          y: "top",
          textAlign: "center",
          textStyle: {
            fontSize: 25,
            fontFamily: "Courier New"
          }
        },
        xAxis: {
          type: "time",
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
          min: "35",
          max: "39",
          splitNumber: "8"
        },
        tooltip: { trigger: "axis" },
        series: [
          {
            symbol: "circle",
            symbolSize: 10,
            name: "",
            data: [
              ["2020/4/1", 35.5],
              ["2020/4/2", 36.8],
              ["2020/4/3", 36.4],
              ["2020/4/4", 36.5],
              ["2020/4/5", 37.0],
              ["2020/4/6", 37.5],
              ["2020/4/7", 38.3]
            ],
            type: "line",
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
      }
    };
  },
  methods: {
    handleCheck(index, row) {
      this.dialogTableVisible = true;
      console.log(index);
      console.log(row);
      this.option.title.text = row.name + "体温情况";
      this.option.series[0].name = row.name;
    },
    handleDelete(index, row) {},
    drawLine() {
      let myChart = echarts.init(this.$refs.charts);
      myChart.setOption(this.option);
    },
    open() {
      this.$nextTick(() => {
        this.drawLine();
      });
    }
  },
  mounted() {}
};
</script>