<template>
  <div>
    <div style="margin-bottom:30px">
      <el-row>
        <el-col :span="6" align="center">
          <span style="font-size:20px;">
            <b>姓名：</b>
          </span>
          <el-input v-model="username" placeholder="请输入姓名" style="width:50%"></el-input>
        </el-col>
        <el-col :span="6" align="center">
          <span style="font-size:20px;">
            <b>身份证号：</b>
          </span>
          <el-input v-model="idcard" placeholder="请输入身份证号" style="width:50%"></el-input>
        </el-col>
        <el-col :span="6" align="center">
          <el-button size="medium" type="primary" icon="el-icon-search" @click="search">查询</el-button>
          <el-button size="medium" type="warning" icon="el-icon-circle-plus-outline" @click="add">添加</el-button>
        </el-col>
        <!-- 添加记录 -->
        <el-dialog title="添加记录" :visible.sync="dialogFormVisible" align="center">
          <el-form
            ref="form"
            :model="sizeForm"
            label-width="80px"
            size="small"
            style="width:50%"
            label-position="left"
            align="left"
          >
            <el-form-item label="注册日期">
              <el-col :span="11">
                <el-date-picker
                  type="date"
                  placeholder="选择日期"
                  v-model="sizeForm.date"
                  style="width: 100%;"
                ></el-date-picker>
              </el-col>
            </el-form-item>
            <el-form-item label="证件号">
              <el-input v-model="sizeForm.idCard" style="width:50%"></el-input>
            </el-form-item>
            <el-form-item label="姓名">
              <el-input v-model="sizeForm.name" style="width:50%"></el-input>
            </el-form-item>
            <el-form-item label="住户地址">
              <el-input v-model="sizeForm.address" style="width:80%"></el-input>
            </el-form-item>
            <el-form-item size="large">
              <el-button type="primary" @click="onSubmit">保存</el-button>
              <el-button @click="dialogFormVisible = false">取消</el-button>
            </el-form-item>
          </el-form>
        </el-dialog>
      </el-row>
    </div>
    <el-table size="small" :data="tableData" height="540" stripe style="width: 100%">
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
      sizeForm: {},
      dialogFormVisible: false,
      dialogTableVisible: false,
      username: "",
      idcard: "",
      tableData: [],
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
            data: [],
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
      var that = this;
      this.dialogTableVisible = true;
      this.option.title.text = row.username + "体温情况";
      this.option.series[0].name = row.username;
      this.$axios
        .get("http://localhost:8880/getInInfo/getTemps/" + row.id_card)
        .then(resp => {
          console.log(resp.data);
          var dataArr = resp.data;
          var chartsData = [];
          for (var index in dataArr) {
            var item = [];
            console.log(dataArr[index]);
            item.push(dataArr[index].in_time);
            item.push(dataArr[index].temperature);
            chartsData.push(item);
          }
          echarts.init(this.$refs.charts).setOption({
            series: [
              {
                symbol: "circle",
                symbolSize: 10,
                name: "",
                data: chartsData,
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
          });
        });
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
    search() {
      var that = this;
      var data = {
        username: this.username,
        idcard: this.idcard
      };
      this.$axios
        .post("http://localhost:8880/getUserInfo/getUserByUsernameOrId", data)
        .then(resp => {
          that.tableData = resp.data;
        });
    },
    add() {
      this.dialogFormVisible = true;
    },
    onSubmit() {
      var data = this.sizeForm;
      var that = this;
      this.$axios
        .post("http://localhost:8880/getUserInfo/addUser", data)
        .then(resp => {
          if (resp.data) {
            that.sizeForm = {};
            that.dialogFormVisible = false;
            that.getInfos();
          }
        });
    },
    getInfos() {
      var that = this;
      this.$axios
        .get("http://localhost:8880/getUserInfo/getAllUsers")
        .then(resp => {
          that.tableData = resp.data;
        });
    }
  },
  created() {
    this.getInfos();
  }
};
</script>