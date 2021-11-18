<template>
  <div class="container_login">
    <div class="login_box">
      <el-form
        ref="LogimFormRef"
        class="login_form"
        :model="form"
        :rules="Loginrules"
      >
        <el-form-item prop="username">
          <!-- 用户名 -->
          <el-input
            v-model="form.username"
            prefix-icon="iconfont icon-user"
          ></el-input>
        </el-form-item>
        <!-- 密码 -->
        <el-form-item prop="password">
          <el-input
            v-model="form.password"
            prefix-icon="iconfont icon-3702mima"
            type="password"
          ></el-input>
        </el-form-item>
        <!-- 按钮 -->
        <el-form-item class="btn">
          <el-button type="primary" @click="login">登录</el-button>
          <el-button type="info" @click="resetLogin">重置</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      // 这个登录数据绑定对象
      form: {
        username: "admin",
        password: "123456",
      },
      //   表单验证规则
      Loginrules: {
        //   验证用户名是否合法
        username: [
          { required: true, message: "请输入用户名", trigger: "blur" },
          {
            min: 3,
            max: 10,
            message: "长度在 3 到 10 个字符",
            trigger: "blur",
          },
        ],
        // 验证密码是否合法
        password: [
          { required: true, message: "请输入密码", trigger: "blur" },
          {
            min: 6,
            max: 15,
            message: "长度在 6 到 15 个字符",
            trigger: "blur",
          },
        ],
      },
    };
  },
  methods: {
    resetLogin() {
      this.$refs.LogimFormRef.resetFields();
    },
    login() {
      this.$refs.LogimFormRef.validate(async (vaild) => {
        if (!vaild) return;
        const { data: res } = await this.$http.post("login", this.form);
        if (res.meta.status !== 200) {
          return this.$message.error("登录失败");
        }
        this.$message.success("登录成功");
        console.log(res);
        window.sessionStorage.setItem("token", res.data.token);
        this.$router.push("/home");
      });
    },
  },
};
</script>
<style lang="less" scoped>
.container_login {
  position: relative;
  width: 100%;
  height: 100%;
  background: #2b4b6b;
  .login_box {
    width: 450px;
    height: 300px;
    background: #fff;
    border-radius: 3px;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    .login_form {
      position: absolute;
      bottom: 0;
      width: 100%;
      padding: 0 20px;
      box-sizing: border-box;
      .btn {
        display: flex;
        justify-content: flex-end;
      }
    }
  }
}
</style>