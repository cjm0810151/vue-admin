<template>
  <div id="login">
    <div class="login-wrap">
      <ul class="menu-tab">
        <li
          :class="{ current: currentIndex == index }"
          v-for="(item, index) in menuTabs"
          :key="item.id"
          @click="toggleMenu(item, index)"
        >
          {{ item.txt }}
        </li>
      </ul>
      <!-- 表单开始 -->
      <el-form
        :model="loginForm"
        status-icon
        :rules="rules"
        ref="loginForm"
        size="medium"
        class="login-form"
      >
        <el-form-item prop="username" class="form-item">
          <label>邮箱</label>
          <el-input
            type="text"
            v-model="loginForm.username"
            autocomplete="off"
          ></el-input>
        </el-form-item>
        <el-form-item prop="password" class="form-item">
          <label>密码</label>
          <el-input
            type="password"
            v-model="loginForm.password"
            maxlength="20"
            minlength="6"
            autocomplete="off"
          ></el-input>
        </el-form-item>
        <el-form-item
          v-if="model === 'register'"
          prop="password2"
          class="form-item"
        >
          <label>重复密码</label>
          <el-input
            type="password"
            v-model="loginForm.password2"
            maxlength="20"
            minlength="6"
            autocomplete="off"
          ></el-input>
        </el-form-item>
        <el-form-item prop="code" class="form-item">
          <label>验证码</label>
          <el-row :gutter="10">
            <el-col :span="15">
              <el-input
                v-model="loginForm.code"
                maxlength="6"
                minlength="6"
              ></el-input>
            </el-col>
            <el-col :span="9">
              <el-button type="success" @click="getCode()" class="block"
                >获取验证码</el-button
              >
            </el-col>
          </el-row>
        </el-form-item>
        <el-form-item>
          <el-button
            type="danger"
            @click="submitForm('loginForm')"
            class="login-btn block"
            >提交</el-button
          >
        </el-form-item>
      </el-form>
      <!-- 表单结束 -->
    </div>
  </div>
</template>

<script>
import {
  stripscript,
  validateEmail,
  validatePass,
  validateVCode
} from "@/utils/validate";
export default {
  name: "Login",
  data() {
    var validateUsername = (rule, value, callback) => {
      if (value === "") {
        callback(new Error("请输入邮箱"));
      } else if (!validateEmail(value)) {
        callback(new Error("邮箱格式不正确"));
      } else {
        callback();
      }
    };
    var validatePassword = (rule, value, callback) => {
      this.loginForm.password = stripscript(value);
      value = this.loginForm.password;
      if (value === "") {
        callback(new Error("请输入密码"));
      } else if (!validatePass(value)) {
        callback(new Error("密码为6至20位的数字加字母"));
      } else {
        callback();
      }
    };
    var validatePassword2 = (rule, value, callback) => {
      this.loginForm.password2 = stripscript(value);
      value = this.loginForm.password2;
      if (value === "") {
        callback(new Error("请再次输入密码"));
      } else if (value !== this.loginForm.password) {
        callback(new Error("密码不一致"));
      } else {
        callback();
      }
    };
    var validateCode = (rule, value, callback) => {
      this.loginForm.code = stripscript(value);
      value = this.loginForm.code;
      if (value === "") {
        callback(new Error("请输入验证码"));
      } else if (!validateVCode(value)) {
        callback(new Error("验证码格式有误"));
      } else {
        callback();
      }
    };
    return {
      currentIndex: 0,
      model: "login",
      menuTabs: [
        { id: 1, txt: "登录", type: "login" },
        { id: 2, txt: "注册", type: "register" }
      ],
      loginForm: {
        username: "",
        password: "",
        password2: "",
        code: ""
      },
      rules: {
        username: [{ validator: validateUsername, trigger: "blur" }],
        password: [{ validator: validatePassword, trigger: "blur" }],
        password2: [{ validator: validatePassword2, trigger: "blur" }],
        code: [{ validator: validateCode, trigger: "blur" }]
      }
    };
  },
  methods: {
    toggleMenu(data, index) {
      this.currentIndex = index;
      this.model = data.type;
    },
    submitForm(formName) {
      this.$refs[formName].validate(valid => {
        if (valid) {
          alert("submit!");
        } else {
          console.log("error submit!!");
          return false;
        }
      });
    },
    getCode() {}
  }
};
</script>

<style lang="scss" scoped>
#login {
  height: 100vh;
  background-color: #344a5f;
}
.login-wrap {
  width: 330px;
  margin: auto;
}
.menu-tab {
  text-align: center;
  li {
    display: inline-block;
    width: 88px;
    line-height: 36px;
    font-size: 14px;
    color: #fff;
    border-radius: 2px;
    cursor: pointer;
  }
  .current {
    background-color: rgba(0, 0, 0, 0.1);
  }
}
.login-form {
  margin-top: 29px;
  label {
    display: block;
    margin-bottom: 3px;
    font-size: 14px;
    color: #fff;
  }
  .form-item {
    margin-bottom: 13px;
  }
  .block {
    display: block;
    width: 100%;
  }
  .login-btn {
    margin-top: 19px;
  }
}
</style>
