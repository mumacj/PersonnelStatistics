<template>
  <div>
    <div style="margin-bottom:30px">
      <el-row>
        <!-- <el-col :span="12" align="center">
          <span style="font-size:20px;"><b>起止时间：</b></span>
          <el-date-picker
            v-model="dateForSearch"
            type="datetimerange"
            align="right"
            start-placeholder="开始日期"
            end-placeholder="结束日期"
            :default-time="['12:00:00', '08:00:00']"
          ></el-date-picker>
        </el-col> -->
        <el-col :span="6" align="center">
          <span style="font-size:20px;"><b>姓名：</b></span>
          <el-input v-model="username" placeholder="请输入姓名" style="width:50%"></el-input>
        </el-col>
        <el-col :span="6" align="center">
          <span style="font-size:20px;"><b>身份证号：</b></span>
          <el-input v-model="idcard" placeholder="请输入身份证号" style="width:50%"></el-input>
        </el-col>
        <el-col :span="6" align="center">
          <el-button
            size="medium"
            type="primary"
            icon="el-icon-search"
            @click="search"
          >查询</el-button>
          <el-button
            size="medium"
            type="warning"
            icon="el-icon-circle-plus-outline"
            @click="add"
          >添加</el-button>
        </el-col>
      </el-row>
    </div>
    <el-table size="small" :data="tableData" height="54 0" stripe style="width: 100%">
      <el-table-column prop="registe_date" label="注册日期" width="180" align="center"></el-table-column>
      <el-table-column prop="id_card" label="身份证号" width="180" align="center"></el-table-column>
      <el-table-column prop="username" label="姓名" width="180" align="center"></el-table-column>
      <el-table-column prop="address" label="住户地址" align="center"></el-table-column>
      <el-table-column prop="in_times" label="出入小区/次" align="center"></el-table-column>
      <el-table-column label="操作" align="center">
        <template slot-scope="scope">
          <el-button size="mini" type="primary" @click="handleCheck(scope.$index, scope.row)">查看</el-button>
          <!-- <el-button size="mini" type="warning" @click="handleEdit(scope.$index, scope.row)">编辑</el-button> -->
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
      username:"",
      idcard:"",
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
      this.option.title.text = row.username + "体温情况";
      this.option.series[0].name = row.username;
      this.$axios
        .get("http://localhost:8880/getUserInfo/getAllUsers")
        .then(resp => {
          that.tableData = resp.data
        })
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
    },
    search(){
      var that = this;
      var data = {
        username:this.username,
        idcard: this.idcard
        };
      this.$axios
        .post("http://localhost:8880/getUserInfo/getUserByUsernameOrId",data)
        .then(resp => {
          that.tableData = resp.data
        })
    },
    add(){

    }
  },
  created(){
    var that = this;
     this.$axios
        .get("http://localhost:8880/getUserInfo/getAllUsers")
        .then(resp => {
          that.tableData = resp.data
        })
  }
};
</script>