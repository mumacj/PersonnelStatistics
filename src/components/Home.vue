<template>
  <div>
    <head-top></head-top>
    <el-container>
      <el-aside>
        <el-menu
          style="height:700px;background-color:#e2e2e2"
          default-active="1"
          class="el-menu-vertical-demo"
          @select="handleSelect"
        >
          <el-menu-item index="1">
            <i class="el-icon-user-solid"></i>
            <span slot="title">业主信息</span>
          </el-menu-item>
          <el-menu-item index="2">
            <i class="el-icon-odometer"></i>
            <span slot="title">出入登记</span>
          </el-menu-item>
          <el-menu-item index="3">
            <i class="el-icon-s-data"></i>
            <span slot="title">体温概览</span>
          </el-menu-item>
          <el-menu-item index="4">
            <i class="el-icon-data-line"></i>
            <span slot="title">实时疫情</span>
          </el-menu-item>
          <el-menu-item index="5">
            <i class="el-icon-setting"></i>
            <span slot="title">意见反馈</span>
          </el-menu-item>
        </el-menu>
      </el-aside>
      <el-main>
        <client-information v-if="this.index == 1"></client-information>
        <in-register v-else-if="this.index == 2"></in-register>
        <temp-general-view v-else-if="this.index == 3"></temp-general-view>
        <new-pneumonia v-else-if="this.index == 4"></new-pneumonia>
        <suggestion v-else-if="this.index == 5"></suggestion>
      </el-main>
    </el-container>

    <foot-btm></foot-btm>
  </div>
</template>

<script>
import Header from "@/components/Header";
import Footer from "@/components/Footer";
import ClientInformation from "@/components/ClientInformation";
import InRegister from "@/components/InRegister";
import TempGeneralView from "@/components/TempGeneralView";
import NewPneumonia from "@/components/NewPneumonia"
import Suggestion from "@/components/Suggestion"

export default {
  data() {
    return {
      index: 1
    };
  },
  components: {
    headTop: Header,
    footBtm: Footer,
    clientInformation: ClientInformation,
    inRegister: InRegister,
    tempGeneralView: TempGeneralView,
    newPneumonia: NewPneumonia,
    suggestion: Suggestion
  },
  methods: {
    handleSelect(index, indexPath) {
      this.index = index;
    }
  }
};
</script>

<style scoped>
.el-main {
  height: 700px;
}
.el-aside {
  width: "200px";
}
</style>