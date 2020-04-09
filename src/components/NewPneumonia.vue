<template>
  <div>
    <div id="temperaturePie" ref="charts" style="width:800px;height:640px;display:inline-block"></div>
    <div style="display:inline-block;width:700px">
      <el-card shadow="always" style="height:300px;margin-bottom:30px">
        <span>
          <b style="font-size:30px">国内疫情</b>
          <el-alert
            :title="'数据更新至'+ lastUpdateTime "
            type="info"
            :closable="false"
            style="display:inline-block;width:500px;padding:0 0!important"
          ></el-alert>
        </span>
        <el-row :gutter="10" style="margin-top:20px">
          <el-col :span="8">
            <el-card shadow="never" class="card" align="center" style="background-color:rgb(255,240,241)" >累计确诊 
                <br> <span style="padding-top:10px;color:rgb(204,30,30);font-size:30px;line-height:40px"><b>{{chinaTotal.confirm}}</b></span>
                <br> 较昨日<span style="padding-top:10px;color:rgb(204,30,30);font-size:15px;"><b><span v-if="chinaAdd.confirm > 0">+</span>{{chinaAdd.confirm}}</b></span>
                </el-card>
          </el-col>
          <el-col :span="8">
            <el-card shadow="never" class="card" align="center" style="background-color:rgb(241,248,244)">累计治愈
                <br> <span style="padding-top:10px;color:rgb(23,139,80);font-size:30px;line-height:40px"><b>{{chinaTotal.heal}}</b></span>
                <br> 较昨日<span style="padding-top:10px;color:rgb(23,139,80);font-size:15px;"><b><span v-if="chinaAdd.heal > 0">+</span>{{chinaAdd.heal}}</b></span>
                </el-card>
          </el-col>
          <el-col :span="8">
            <el-card shadow="never" class="card" align="center" style="background-color:rgb(243,246,248)">累计死亡
                <br> <span style="padding-top:10px;color:rgb(78,90,101);font-size:30px;line-height:40px"><b>{{chinaTotal.dead}}</b></span>
                <br> 较昨日<span style="padding-top:10px;color:rgb(78,90,101);font-size:15px;"><b><span v-if="chinaAdd.dead > 0">+</span>{{chinaAdd.dead}}</b></span>
                </el-card>
                
          </el-col>
        </el-row>
        <el-row :gutter="10" style="margin-top:20px">
          <el-col :span="8">
            <el-card shadow="never" class="card" align="center" style="background-color:rgb(255,240,241)">现有确诊
                <br> <span style="padding-top:10px;color:rgb(242,58,59);font-size:30px;line-height:40px"><b>{{chinaTotal.nowConfirm}}</b></span>
                <br> 较昨日<span style="padding-top:10px;color:rgb(242,58,59);font-size:15px;"><b><span v-if="chinaAdd.nowConfirm > 0">+</span>{{chinaAdd.nowConfirm}}</b></span></el-card>
          </el-col>
          <el-col :span="8">
            <el-card shadow="never" class="card" align="center" style="background-color:rgb(250,242,246)">无症状感染者
                <br> <span style="padding-top:10px;color:rgb(202,63,129);font-size:30px;line-height:40px"><b>{{chinaTotal.noInfect}}</b></span>
                <br> 较昨日<span style="padding-top:10px;color:rgb(202,63,129);font-size:15px;"><b><span v-if="chinaAdd.noInfect > 0">+</span>{{chinaAdd.noInfect}}</b></span></el-card>
          </el-col>
          <el-col :span="8">
            <el-card shadow="never" class="card" align="center" style="background-color:rgb(252,244,240)">境外输入
                <br> <span style="padding-top:10px;color:rgb(240,89,38);font-size:30px;line-height:40px"><b>{{chinaTotal.importedCase}}</b></span>
                <br> 较昨日<span style="padding-top:10px;color:rgb(240,89,38);font-size:15px;"><b><span v-if="chinaAdd.importedCase > 0">+</span>{{chinaAdd.importedCase}}</b></span></el-card>
          </el-col>
        </el-row>
        
      </el-card>
      <el-card shadow="always" style="height:300px"><span>
          <b style="font-size:30px">海外疫情</b>
          <el-alert
            :title="'数据更新至'+ globalStatis.lastUpdateTime "
            type="info"
            :closable="false"
            style="display:inline-block;width:500px;padding:0 0!important"
          ></el-alert>
        </span>
        <el-row :gutter="10" style="margin-top:20px">
          <el-col :span="8">
            <el-card shadow="never" class="card" align="center" style="background-color:rgb(255,240,241)" >累计确诊 
                <br> <span style="padding-top:10px;color:rgb(204,30,30);font-size:30px;line-height:40px"><b>{{globalStatis.confirm}}</b></span>
                <br> 较昨日<span style="padding-top:10px;color:rgb(204,30,30);font-size:15px;"><b><span v-if="globalStatis.confirmAdd > 0">+</span>{{globalStatis.confirmAdd}}</b></span>
                </el-card>
          </el-col>
          <el-col :span="8">
            <el-card shadow="never" class="card" align="center" style="background-color:rgb(241,248,244)">累计治愈
                <br> <span style="padding-top:10px;color:rgb(23,139,80);font-size:30px;line-height:40px"><b>{{globalStatis.heal}}</b></span>
                <br> 较昨日<span style="padding-top:10px;color:rgb(23,139,80);font-size:15px;"><b><span v-if="globalStatis.healAdd > 0">+</span>{{globalStatis.healAdd}}</b></span>
                </el-card>
          </el-col>
          <el-col :span="8">
            <el-card shadow="never" class="card" align="center" style="background-color:rgb(243,246,248)">累计死亡
                <br> <span style="padding-top:10px;color:rgb(78,90,101);font-size:30px;line-height:40px"><b>{{globalStatis.dead}}</b></span>
                <br> 较昨日<span style="padding-top:10px;color:rgb(78,90,101);font-size:15px;"><b><span v-if="globalStatis.deadAdd > 0">+</span>{{globalStatis.deadAdd}}</b></span>
                </el-card>
          </el-col>
        </el-row>
        <el-row :gutter="10" style="margin-top:20px">
            
          <el-col :span="8" :offset="8">
            <el-card shadow="never" class="card" align="center" style="background-color:rgb(255,240,241)">现有确诊
                <br> <span style="padding-top:10px;color:rgb(242,58,59);font-size:30px;line-height:40px"><b>{{globalStatis.nowConfirm}}</b></span>
                <br> 较昨日<span style="padding-top:10px;color:rgb(242,58,59);font-size:15px;"><b><span v-if="globalStatis.nowConfirmAdd > 0">+</span>{{globalStatis.nowConfirmAdd}}</b></span></el-card>
          </el-col>
          
        </el-row>
        </el-card>
    </div>
  </div>
</template>

<script>
var echarts = require("echarts");
import "echarts/map/js/china.js";
export default {
  data() {
    return {
        globalStatis:{},
      lastUpdateTime: "",
      chinaTotal:{

      },
      chinaAdd:{

      },
      option: {
        title: {
          text: "全国实时疫情统计",
          left: "center",
          textStyle: {
            color: "#2c343c",
            fontSize: 30
          }
        },

        tooltip: {
          trigger: "item"
        },
        visualMap: {
          type: "piecewise",
          pieces: [
            {
              min: 10000,
              max: 1000000,
              label: "大于等于10000人",
              color: "rgb(102,2,8)"
            },
            {
              min: 1000,
              max: 9999,
              label: "确诊1000-9999人",
              color: "rgb(140,13,13)"
            },
            {
              min: 100,
              max: 999,
              label: "确诊100-999人",
              color: "rgb(204,41,41)"
            },
            {
              min: 10,
              max: 99,
              label: "确诊10-99人",
              color: "rgb(255,123,105)"
            },
            { min: 1, max: 9, label: "确诊1-9人", color: "rgb(255,170,133)" }
          ],
          color: ["#E0022B", "#E09107", "#A3E00B"]
        },
        toolbox: {
          show: true,
          orient: "vertical",
          left: "right",
          top: "center",
          feature: {
            mark: { show: true },
            dataView: { show: true, readOnly: false },
            restore: { show: true },
            saveAsImage: { show: true }
          }
        },
        roamController: {
          show: true,
          left: "right",
          mapTypeControl: {
            china: true
          }
        },
        series: [
          {
            name: "累计确诊人数",
            type: "map",
            mapType: "china",
            roam: false,
            label: {
              show: true,
              color: "rgb(249, 249, 249)"
            },
            data: []
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
    getData() {
      var that = this;
      this.$axios
        .get("/getOnsInfo?name=disease_h5")
        .then(resp => {
          var dataArr = [];
          var data = JSON.parse(resp.data.data);

          that.lastUpdateTime = data.lastUpdateTime;
          that.chinaTotal = data.chinaTotal;
          that.chinaAdd = data.chinaAdd;
          data.areaTree[0].children.forEach(province => {
            var temp = {};
            temp.name = province.name;
            temp.value = province.total.confirm;
            dataArr.push(temp);
          });

          echarts.init(this.$refs.charts).setOption({
            series: [
              {
                name: "累计确诊人数",
                type: "map",
                mapType: "china",
                roam: false,
                label: {
                  show: true,
                  color: "rgb(249, 249, 249)"
                },
                data: dataArr
              }
            ]
          });
        })
        .catch(resp => {
          console.log(resp);
        });
    },
    //海外数据
    getForeignData(){
        var that = this;
      this.$axios
        .get("/getOnsInfo?name=disease_foreign")
        .then(resp => {
          var dataArr = [];
          var data = JSON.parse(resp.data.data);

          that.globalStatis = data.globalStatis;
        })
        .catch(resp => {
          console.log(resp);
        });
    }
  },
  mounted() {
    this.$nextTick(() => {
      this.drawLine();
      this.getData();
      this.getForeignData();
    });
  }
};
</script>
<style>
.card{
    display:inline-block;
    width:200px;
    height:80px;
    border-radius: 20px;
    border: none;
}
.card .el-card__body{
    padding-top: 0px;
}
</style>