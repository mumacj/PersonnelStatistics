<template>
  <div>
    <div style="margin-bottom:30px">
      <el-row>
        <el-col :span="12" align="center">
          <span style="font-size:20px;"><b>起止时间：</b></span>
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
          <span style="font-size:20px;"><b>姓名：</b></span>
          <el-input v-model="input" placeholder="请输入内容" style="width:50%"></el-input>
        </el-col>
        <el-col :span="6" align="center">
          <el-button
            size="medium"
            type="primary"
            icon="el-icon-search"
          >查询</el-button>
          <el-button
            size="medium"
            type="warning"
            icon="el-icon-circle-plus-outline"
          >添加</el-button>
        </el-col>
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
          <span v-show="scope.row.readonly" @dblclick="change(scope.row)">{{scope.row.time}}</span>
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
      <el-table-column prop="healthMark" label="健康码" align="center">
        <template slot-scope="scope">
          <el-switch
            style="display: block;"
            v-model="scope.row.healthMark"
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
      dateForSearch:"",
      input: "",
      tableData: [
        {
          readonly: true,
          dateTime: "",
          time: "8:20:30",
          name: "王小虎",
          address: "上海市普陀区金沙江路 1516 弄",
          idCard: "339005123456789123",
          tempNow: "37.5",
          wheLeave: "否",
          healthMark: true
        }
      ]
    };
  },
  methods: {
    formatDate(date) {
      if (!date) {
        this.$alert("请选择时间", "提示", {
          confirmButtonText: "确定",
        });

        return;
      }
      var h = date.getHours();
      var m = date.getMinutes();
      var s = date.getSeconds();

      if (h < 10) {
        h = "0" + h;
      }
      if (m < 10) {
        m = "0" + m;
      }
      if (s < 10) {
        s = "0" + s;
      }
      return h + ":" + m + ":" + s;
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
    },
    handleDelete() {},
    change(row) {
      row.readonly = false;
    }
  }
};
</script>