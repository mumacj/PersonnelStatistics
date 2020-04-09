<template>
    <el-header><span>小区人员流动管理系统</span>
    <iframe width="280" scrolling="no" height="25" frameborder="0" allowtransparency="true" src="http://i.tianqi.com/index.php?c=code&id=34&icon=1&num=3"></iframe>
      <el-dropdown style="float:right">
        <el-button >
          <i
            style="font-weight: bold; font-size: 20px"
            class="el-icon-s-home"
          ></i>
          username<i class="el-icon-arrow-down el-icon--right"></i>
        </el-button>
        <el-dropdown-menu slot="dropdown">
            <el-dropdown-item command="edit">修改信息</el-dropdown-item>
          <el-dropdown-item command="logout">退出登录</el-dropdown-item>
        </el-dropdown-menu>
      </el-dropdown>
    </el-header>
</template>


<style scoped>
.el-header{
  height: 100px !important;
  background-color: #008b8b;
  color: beige;
  text-align: left;
  line-height: 100px;
  font-size: 50px;
  font-style: italic;
  font-family: cursive;
}

</style>

<script>
export default {
    name: "Header"
}
</script>