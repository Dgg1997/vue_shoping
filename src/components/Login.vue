<template>
    <div class="login_container">
      <div class="login_box">
        <!--头像区域-->
        <div class="avatar_box">
          <img src="../assets/logo.png">
        </div>
        <!--表单区域-->
        <el-form ref="loginFormRef" :model="loginForm" :rules="loginFormRules" class="login_form" label-width="0px">
          <el-form-item prop="username">
            <el-input v-model="loginForm.username" prefix-icon="iconfont icon-user"></el-input>
          </el-form-item>
          <el-form-item prop="password">
            <el-input type="password" v-model="loginForm.password" prefix-icon="iconfont icon-3702mima"></el-input>
          </el-form-item>
          <el-form-item class="btn">
            <el-button type="primary" @click="login">登录</el-button>
            <el-button type="info" @click="resetLoginForm">重置</el-button>
          </el-form-item>
        </el-form>
      </div>
    </div>
</template>

<script>
export default {
  name: 'Login',
  data () {
    return {
      // 默认是一个空对象
      loginForm: {
        username: 'admin',
        password: '123456'
      },
      // 这是表单的验证规则
      loginFormRules: {
        username: [
          { required: true, message: '请输入登录名称', trigger: 'blur' },
          { min: 3, max: 10, message: '长度在3-10个字符', trigger: 'blur' }
        ],
        password: [
          { required: true, message: '请输入登录密码', trigger: 'blur' },
          { min: 6, max: 15, message: '长度在6-15个字符', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    // 重置按钮重置表单
    resetLoginForm () {
      this.$refs.loginFormRef.resetFields()
    },
    login () {
      this.$refs.loginFormRef.validate(async valid => {
        console.log(valid)
        if (!valid) return
        const { data: res } = await this.$http.post('login', this.loginForm)
        console.log(res)
        if (res.meta.status !== 200) return this.$message.error('登录失败')
        this.$message.success('登录成功')
        // 登录成功后将token保存到客户端sessionStorage
        window.sessionStorage.setItem('token', res.data.token)
        this.$router.push('/home')
      })
    }
  }
}
</script>

<style lang="less" scoped>
  .login_container{
    background-color: #2b4b6b;
    height: 100%;
  }
  .login_box{
    width: 450px;
    height: 300px;
    background-color: #fff;
    border-radius: 3px;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%,-50%);
  }
  .avatar_box{
    width: 100px;
    height: 100px;
    border: 1px solid #eee;
    border-radius: 50%;
    padding: 10px;
    position: absolute;
    left: 50%;
    transform: translate(-50%,-50%);
    background-color: #fff;
    img{
      width: 100%;
      height: 100%;
      border-radius: 50%;
      background-color: #eee;
      box-shadow: 0 0 10px #ddd;
    }
  }
  .login_form{
    position: absolute;
    bottom: 0;
    width: 100%;
    padding: 0 20px;
    box-sizing: border-box;
  }
  .btn{
    display: flex;
    justify-content: flex-end;
  }
</style>
