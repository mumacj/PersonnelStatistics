<template>
  <div class="login-page">
    <head-top></head-top>
    <el-main>
      <div class="login-form-div">
        <el-form
          :model="ruleForm"
          status-icon
          ref="ruleForm"
          :rules="rules"
          label-width="100px"
          class="login-form"
          label-position="left"
        >
          <el-form-item label="用户名" prop="username">
            <el-input
              type="text"
              v-model="ruleForm.username"
              autocomplete="off"
              placeholder="请输入用户名"
            ></el-input>
          </el-form-item>
          <el-form-item label="密码" prop="password">
            <el-input
              type="password"
              v-model="ruleForm.password"
              autocomplete="off"
              placeholder="请输入密码"
            ></el-input>
          </el-form-item>
          <el-form-item>
            <el-button type="primary" @click="submitForm('ruleForm')">登录</el-button>
            <el-button @click="resetForm('ruleForm')">重置</el-button>
          </el-form-item>
        </el-form>
      </div>
    </el-main>
    <foot-btm></foot-btm>
  </div>
</template>


<script>
import Header from "@/components/Header";
import Footer from "@/components/Footer";
export default {
  name: "Login",
  components: {
    'headTop': Header,
    'footBtm':Footer
  },
  data() {
    var validateUser = (rule, value, callback) => {
      if (value === "") {
        callback(new Error("请输入用户名"));
      } else {
        callback();
      }
    };
    var validatePass = (rule, value, callback) => {
      if (value === "") {
        callback(new Error("请输入密码"));
      } else {
        callback();
      }
    };
    return {
      ruleForm: {
        username: "",
        password: ""
      },
      rules: {
        username: [{ validator: validateUser, trigger: "blur" }],
        password: [{ validator: validatePass, trigger: "blur" }]
      }
    };
  },
  methods: {
    submitForm(formName) {
      this.$refs[formName].validate(valid => {
        if (valid) {
          if(this.ruleForm.username == "admin" && this.ruleForm.password == "admin"){
            this.$router.push({ path: "/home" });
          }else{
            alert("用户名或密码错误");
          }
          
        } else {
          console.log("error submit!!");
          return false;
        }
      });
    },
    resetForm(formName) {
      this.$refs[formName].resetFields();
    }
  }
};
</script>

<style scoped>
/* .el-header,
.el-footer {
  height: 85px !important;
  background-color: #008b8b;
  color: beige;
  text-align: left;
  line-height: 85px;
  font-size: 50px;
  font-style: italic;
  font-family: cursive;
}*/
.login-page .el-main {
  height: 700px;
  background-image: url("../assets/xqbjt.jpg");
  background-repeat: no-repeat;
} 
.login-page .login-form-div {
  /* border: 1px solid black; */
  padding: 40px;
  width: 20%;
  margin: auto;
  padding-top: 240px;
  /* background-color: #f56c6c; */
}
.login-page .login-form {
  margin: auto;
}
</style>
<style>
.el-form-item__label {
  color: #f2f6fc !important;
  font-size: 20px;
}
</style>