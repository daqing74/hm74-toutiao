<template>
<div class='login-container'>
<el-card class='login-box'>
<img src='../../assets/images/logo_index.png'>

<el-form ref="loginForm" :status-icon="true" :model="loginForm" :rules="loginRules">

<el-form-item prop="mobile">
<el-input  v-model="loginForm.mobile" placeholder="请输入手机号"  ></el-input>
</el-form-item>

<el-form-item prop="code">
<el-input style="width:60%" v-model="loginForm.code" placeholder="请输入验证码" ></el-input>
<el-button style="float:right">发送验证码</el-button>
</el-form-item>

<el-form-item>
<el-checkbox :value="true">我已阅读</el-checkbox>
</el-form-item>

<el-form-item>
<el-button style="width:100%" type="primary"
@click="login()">登录</el-button>
</el-form-item>

</el-form>
</el-card>
</div>
</template>

<script>
export default {
  data () {
    const checkMobile = (rule, value, callback) => {
      if (/^1[3-9]\d{9}/.test(value)) {
        callback()
      } else {
        callback(new Error('手机号格式不正确'))
      }
    }
    return {
      loginForm: {
        mobile: '',
        code: ''
      },

      loginRules: {
        mobile: [
          { required: true, message: '请输入手机号', trigger: 'blur' },
          { validator: checkMobile, trigger: 'blur' }
        ],
        code: [
          { required: true, message: '请输入验证码', trigger: 'blur' },
          { len: 6, message: '请输入6位数字', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    login () {
      this.$refs.loginForm.validate((valid) => {
        if (valid) {
          this.$http.post('http://ttapi.research.itcast.cn/mp/v1_0/authorizations', this.loginForm).then(res => {
            console.log(res.data)
            this.$router.push('/')
            // 不用router-link用js代码
          }).catch(() => {
            this.$message.error('手机号或验证码失败')
          })
        }
      })
    }
  }
}
</script>

<style scoped lang='less'>
.login-container{
    width:100%;
    height:100%;
    position:absolute;
    left:0;
    top:0;
    background:url(../../assets/images/login_bg.jpg) no-repeat center / cover;

    .login-box{
        width:400px;
        height:350px;
        position:absolute;
        left:50%;
        top:50%;
        transform: translate(-50%,-50%);
        img{
            display:block;
            width:200px;
            margin:10px auto;
        }
    }
}

</style>
