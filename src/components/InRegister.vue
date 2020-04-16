<template>
  <div>
    <div style="margin-bottom:30px">
      <el-row>
        <el-col :span="12" align="center">
          <span style="font-size:20px;">
            <b>起止时间：</b>
          </span>
          <el-date-picker
            v-model="dateForSearch"
            type="datetimerange"
            align="right"
            start-placeholder="开始日期"
            end-placeholder="结束日期"
            :default-time="['12:00:00', '08:00:00']"
          ></el-date-picker>
        </el-col>
        <el-col :span="6" align="center">
          <span style="font-size:20px;">
            <b>姓名：</b>
          </span>
          <el-input v-model="name" placeholder="请输入姓名" style="width:50%"></el-input>
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
            <el-form-item label="证件号">
              <el-input v-model="sizeForm.idCard" style="width:50%"></el-input>
            </el-form-item>
            <el-form-item label="姓名">
              <el-input v-model="sizeForm.name" style="width:50%"></el-input>
            </el-form-item>
            <el-form-item label="时间">
              <el-col :span="11">
                <el-date-picker
                  type="date"
                  placeholder="选择日期"
                  v-model="sizeForm.date"
                  style="width: 100%;"
                ></el-date-picker>
              </el-col>
              <el-col class="line" :span="2">-</el-col>
              <el-col :span="11">
                <el-time-picker placeholder="选择时间" v-model="sizeForm.time" style="width: 100%;"></el-time-picker>
              </el-col>
            </el-form-item>
            <el-form-item label="住户地址">
              <el-input v-model="sizeForm.address" style="width:80%"></el-input>
            </el-form-item>
            <el-form-item label="当前体温">
              <el-input v-model="sizeForm.tempNow" style="width:50%"></el-input>
            </el-form-item>
            <el-form-item label="是否离杭">
              <el-radio-group v-model="sizeForm.wheLeave" size="medium">
                <el-radio border label="是"></el-radio>
                <el-radio border label="否"></el-radio>
              </el-radio-group>
            </el-form-item>
            <el-form-item label="健康码">
              <template>
                <el-switch
                  style="display: block;"
                  v-model="sizeForm.healthCode"
                  active-color="#13ce66"
                  inactive-color="#ff4949"
                  active-text="健康"
                  inactive-text="待观察"
                ></el-switch>
              </template>
            </el-form-item>
            <el-form-item size="large">
              <el-button type="primary" @click="onSubmit">保存</el-button>
              <el-button>取消</el-button>
            </el-form-item>
          </el-form>
        </el-dialog>
      </el-row>
    </div>
    <el-table size="small" :data="tableData" height="540" stripe style="width: 100%">
      <el-table-column prop="time" label="时间" width="220" align="center">
        <template slot-scope="scope">
          <el-date-picker
            style="width:100%"
            type="datetime"
            placeholder="选择进入小区时间"
            v-show="!scope.row.readonly"
            v-model="scope.row.dateTime"
            :readonly="scope.row.readonly"
          ></el-date-picker>
          <span v-show="scope.row.readonly" @dblclick="change(scope.row)">{{scope.row.dateTime}}</span>
        </template>
      </el-table-column>
      <el-table-column prop="name" label="姓名" width="130" align="center">
        <template slot-scope="scope">
          <el-input v-show="!scope.row.readonly" v-model="scope.row.name" placeholder="请输入姓名"></el-input>
          <span v-show="scope.row.readonly" @dblclick="change(scope.row)">{{scope.row.name}}</span>
        </template>
      </el-table-column>
      <el-table-column prop="address" label="住户地址" align="center">
        <template slot-scope="scope">
          <el-input v-show="!scope.row.readonly" v-model="scope.row.address" placeholder="请输入单元门牌号"></el-input>
          <span v-show="scope.row.readonly" @dblclick="change(scope.row)">{{scope.row.address}}</span>
        </template>
      </el-table-column>
      <el-table-column prop="idCard" label="身份证号" align="center">
        <template slot-scope="scope">
          <el-input v-show="!scope.row.readonly" v-model="scope.row.idCard" placeholder="请输入身份证号"></el-input>
          <span v-show="scope.row.readonly" @dblclick="change(scope.row)">{{scope.row.idCard}}</span>
        </template>
      </el-table-column>
      <el-table-column prop="tempNow" label="当前体温/℃" align="center">
        <template slot-scope="scope">
          <el-input v-show="!scope.row.readonly" v-model="scope.row.tempNow" placeholder="请输入当前体温"></el-input>
          <span v-show="scope.row.readonly" @dblclick="change(scope.row)">{{scope.row.tempNow}}</span>
        </template>
      </el-table-column>
      <el-table-column prop="wheLeave" label="是否离杭" align="center"></el-table-column>
      <el-table-column prop="healthCode" label="健康码" align="center">
        <template slot-scope="scope">
          <el-switch
            style="display: block;"
            v-model="scope.row.healthCode"
            active-color="#13ce66"
            inactive-color="#ff4949"
            active-text="健康"
            inactive-text="待观察"
            :disabled="scope.row.readonly"
          ></el-switch>
        </template>
      </el-table-column>
      <el-table-column label="操作" align="center" width="300">
        <template slot-scope="scope">
          <el-button
            size="mini"
            type="primary"
            @click="handleEdit(scope.$index, scope.row)"
            icon="el-icon-edit"
          >编辑</el-button>
          <el-button
            size="mini"
            type="success"
            @click="handleSave(scope.$index, scope.row)"
            icon="el-icon-upload"
          >保存</el-button>
          <el-button
            size="mini"
            type="danger"
            @click="handleDelete(scope.$index, scope.row)"
            icon="el-icon-delete"
          >删除</el-button>
        </template>
      </el-table-column>
    </el-table>
    <el-pagination
      :page-sizes="[10, 20, 30, 40]"
      :page-size="10"
      layout="total, sizes, prev, pager, next, jumper"
      :total="400"
    ></el-pagination>
  </div>
</template>

<script>
export default {
  data() {
    return {
      sizeForm: {},
      dialogFormVisible: false,
      dateForSearch: "",
      name: "",
      tableData: [
        
      ]
    };
  },
  methods: {
    formatDate(date) {
      if (!date) {
        this.$alert("请选择时间", "提示", {
          confirmButtonText: "确定"
        });

        return;
      }
      date = new Date(date)
      console.log(date)
      var y = date.getFullYear();
      var M = date.getMonth() + 1;
      var d = date.getDate();
      var h = date.getHours();
      var m = date.getMinutes();
      var s = date.getSeconds();

      if (M < 10) {
        M = "0" + M;
      }
      if (d < 10) {
        d = "0" + d;
      }
      if (h < 10) {
        h = "0" + h;
      }
      if (m < 10) {
        m = "0" + m;
      }
      if (s < 10) {
        s = "0" + s;
      }
      return y+"-"+ M +"-" + d+" " + h + ":" + m + ":" + s;
    },
    handleEdit(index, row) {
      row.readonly = false;
      console.log(row.time);
    },
    handleSave(index, row) {
      if (!row.readonly) {
        row.time = this.formatDate(row.dateTime);
      }
      row.readonly = true;

      var that = this
      var data  = row;
      this.$axios
        .post("http://localhost:8880/getInInfo/updateInfo",data)
        .then(resp => {
          if(resp.data){
            alert("保存成功");
          }else{
            alert("保存失败");
          }
        })
    },
    handleDelete(index, row) {
      var that = this
      this.$axios
        .post("http://localhost:8880/getInInfo/deleteInfo/"+row.id)
        .then(resp => {
          if(resp){
            that.tableData.splice(index,1);
            alert("删除成功")
          }else{
            alert("删除失败")
          }
        })
    },
    change(row) {
      row.readonly = false;
    },
    add() {
      this.dialogFormVisible = true;
    },
    onSubmit(){
      var data = this.sizeForm;
      var that = this;
      this.$axios
        .post("http://localhost:8880/getInInfo/addInfo",data)
        .then(resp => {
          if(resp.data){
            that.sizeForm = {};
            that.dialogFormVisible = false;
            that.getInfos();
          }
        })
    },
    search(){
      var data = {
        date:this.dateForSearch,
        name:this.name
      }
      var that = this;
      this.$axios
        .post("http://localhost:8880/getInInfo/getInfosByTimeOrName",data)
        .then(resp => {
          that.tableData = resp.data;
        })
    },
    getInfos(){
      var that = this
      this.$axios
        .get("http://localhost:8880/getInInfo/getInfos")
        .then(resp => {
          if(resp.data){
            that.tableData = resp.data
          }
        })
    }
  },
  created(){
    this.getInfos();
  }
};
</script>