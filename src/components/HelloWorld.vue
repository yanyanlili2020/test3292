<template>
  <div id="login">
    <div class="login-wrap">
      <ul class="menu-tab">
        <li v-for="item in menuTab" :class="{ current: item.current }" :key="item.id" @click="toggleMenu(item)">
          {{ item.txt }}
        </li>
      </ul>
      <!-- el表单 -->
      <el-form :model="ruleForm" status-icon :rules="rules" ref="ruleForm" class="login-form" size="medium">
        <el-form-item prop="email" class="item-form">
          <label>邮箱</label>
          <el-input type="text" v-model="ruleForm.email" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item prop="password" class="item-form">
          <label>密码</label>
          <el-input type="password" v-model="ruleForm.password" autocomplete="off" minlength="6" maxlength="20"></el-input>
        </el-form-item>
        <el-form-item prop="checkPass" class="item-form" v-show="model==='register'">
          <label>确认密码</label>
          <el-input type="password" v-model="ruleForm.checkPass" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item prop="code" class="item-form">
          <label>验证码</label>
          <el-row :gutter="10">
            <el-col :span="15">
              <el-input v-model.number="ruleForm.code"></el-input>
            </el-col>
            <el-col :span="9">
              <el-button type="success" @click="submitForm('ruleForm')" class=" block">获取验证码</el-button>
            </el-col>
          </el-row>
        </el-form-item>
        <el-form-item>
          <el-button type="danger" @click="submitForm('ruleForm')" class="login-top block">提交</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>
<script>
import {stripscript,emailscript,pwdscript} from '@/utils/validate';
export default {
  name: 'index',
  data() {
    var validateEmail = (rule, value, callback) => {

      if (value === "") {
        callback(new Error("请输入邮箱"));
      } else if (!emailscript(value)) {
        callback(new Error("请输入正确邮箱"));
      } else {
        callback();
      }
    };
    var validatePassword = (rule, value, callback) => {
      this.ruleForm.password=stripscript(value);
      value=this.ruleForm.password;

      if (value === "") {
        callback(new Error("请输入密码"));
      } else if (!pwdscript(value)) {
        callback(new Error("密码应为6-20位数字加字母!"));
      } else {
        callback();
      }
    };
    var validatePass2 = (rule, value, callback) => {
      if (value === '') {
        callback(new Error('请再次输入密码'));
      } else if (value !== this.ruleForm.password) {
        callback(new Error('两次输入密码不一致!'));
      } else {
        callback();
      }
    };
    var checkCode = (rule, value, callback) => {
      this.ruleForm.code=stripscript(value);
      value=this.ruleForm.code;
      let reg = /^[a-z0-9]{6}$/;
      if (!value) {
        return callback(new Error("请输入验证码"));
      } else if (!reg.test(value)) {
        callback(new Error("验证码格式有误!"));
      } else {
        callback();
      }
    };
    return {
      labelPosition: "top",
      model:'login',
      ruleForm: {
        email: "",
        password: "",
        checkPass: "",
        code: "",
      },
      rules: {
        email: [{validator: validateEmail, trigger: "blur"}],
        password: [{validator: validatePassword, trigger: "blur"}],
        checkPass: [
          { validator: validatePass2, trigger: 'blur' }
        ],
        code: [{validator: checkCode, trigger: "blur"}],
      },
      menuTab: [
        {
          txt: "登录",
          current: true,
          type:'login',
        },
        {
          txt: "注册",
          current: false,
          type:'register',
        },
      ],
      isActive: true,
    };
  },
  mounted() {
  },
  methods: {
    toggleMenu(data) {
      this.menuTab.forEach((el) => {
        el.current = false;
      });
      data.current = true;
      this.model=data.type;
    },
    submitForm(formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          alert("submit!");
        } else {
          console.log("error submit!!");
          return false;
        }
      });
    },
    resetForm(formName) {
      this.$refs[formName].resetFields();
    },
  },
};
</script><!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
@import "../styles/normalize.css";
#login {
  height:           100vh;
  background-color: #344A5F;
  }
.login-wrap {
  width:  330px;
  margin: auto;
  }
.menu-tab {
  text-align: center;
  }
.menu-tab li {
  display:       inline-block;
  width:         88px;
  line-height:   36px;
  font-size:     14px;
  color:         #FFFFFF;
  border-radius: 2px;
  cursor:        pointer;
  }
.menu-tab .current {
  background-color: rgba(0, 0, 0, 0.1);
  }
.login-form {
  margin-top: 29px;
  }
.login-form label {
  text-align: left;
  display:       block;
  margin-bottom: 3px;
  font-size:     14px;
  color:         #FFFFFF;
  }
.login-form .item-form {
  margin-bottom: 13px;
  }
.login-form .block {
  display: block;
  width:   100%;
  }
.login-form .login-top {
  margin-top: 19px;
  }
</style>

