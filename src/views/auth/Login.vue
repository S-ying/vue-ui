<template>
  <div class="loginPage">
    <div class="bg"></div>
    <div class="form">
      <el-row>
          <el-col :span="24">
              <el-form label="" :label-position="labelPosition" label-width="100px" :model="ruleForm" ref="ruleForm" :rules="rules">
                  <el-form-item label="邮 箱" prop="email">
                    <el-input v-model="ruleForm.email" type="text" @keyup.enter.native="submitForm('ruleForm')"></el-input>
                  </el-form-item>
                  <el-form-item label="密 码" style="color: #fff;" prop="password">
                    <el-input v-model="ruleForm.password" type="password" @keyup.enter.native="submitForm('ruleForm')"></el-input> <!-- 封装组件加native -->
                  </el-form-item>
                  <el-form-item class="login_btn">
                    <el-button type="primary" @click="submitForm('ruleForm')">登 录</el-button>
                    <el-button type="info" @click="clearForm('ruleForm')">重 置</el-button>
                  </el-form-item>
              </el-form>
          </el-col>
      </el-row>
    </div>
  </div>
</template>

<script>
import { mapMutations } from 'vuex'
export default {
  name: 'Login',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      isPass: false,
      labelPosition: 'right',
      ruleForm: {
        email: 'sky_cmc@163.com',
        password: 's1120080283'
      },
      rules: {
        email: [
          { required: true, message: '请输入邮箱地址', trigger: 'blur' },
          {
            type: 'email',
            message: '请输入正确的邮箱地址',
            trigger: 'blur,change'
          }
        ],
        password: [{ required: true, message: '请输入密码', trigger: 'blur' }]
      }
    }
  },
  methods: {
    ...mapMutations(['LOGIN']),
    inputBlur () {
      if (!this.email.trim() || !this.password.trim()) {
        this.isPass = false
        return this.$message({
          message: '用户名或密码不能为空',
          type: 'warning'
        })
      }
      this.isPass = true
    },
    submitForm (formName) {
      this.$refs[formName].validate(valid => {
        if (valid) {
          this.$http.login({
            email: this.ruleForm.email,
            password: this.ruleForm.password
          }).then(res => {
            console.log(res)
            if (res.code === 0) {
              this.LOGIN(res.data)
              this.$router.push('/main')
            } else {
              this.$message.error(res.message)
            }
          })
            .catch(err => {
              this.$message.error(err.message)
            })
        }
      })
    },
    clearForm (formName) {
      this.$refs[formName].resetFields()
      this.isPass = false
    }
  }
}
</script>

<style scoped>
html,
body {
  margin: 0;
  padding: 0;
}
.loginPage {
  width: 100%;
  height: 100%;
}
.bg{
  width: 100%;
  height: 100%;
  background-image: url(../../assets/bg_login.jpg);
  background-size: auto;
  background-position: center center;
  filter: blur(3px);
}
.form{
  position: absolute;
  padding: 20px 30px 0 0;
  border-radius: 10px;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: rgba(255, 255, 255,.5);
  display: flex;
  justify-content: center;
  align-items: center;
}
.form_item{
  margin-bottom: 20px;
}
.login_btn{
  text-align: left;
}
</style>
