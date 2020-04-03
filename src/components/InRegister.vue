<template>
  <div>
    <el-table size="small" :data="tableData" height="570" stripe style="width: 100%">
      <el-table-column prop="time" label="时间" width="200" align="center">
        <template slot-scope="scope">
          <el-time-picker
            v-show="!scope.row.readonly"
            style="width:100%"
            v-model="scope.row.dateTime"
            placeholder="选择进入小区时间"
            :readonly="scope.row.readonly"
          ></el-time-picker>

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
      <el-table-column prop="wheLeave" label="是否离杭" align="center">

      </el-table-column>
      <el-table-column prop="healthMark" label="健康码" align="center">
          
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
          healthMark: "健康"
        }
      ]
    };
  },
  methods: {
    formatDate(date) {
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