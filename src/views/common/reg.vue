<template>
  <div class="site-wrapper site-page--login">
    <div class="site-content__wrapper">
      <div class="site-content">
        <div class="brand-info">
          <h2 class="brand-info__text">牙科诊所系统</h2>
          <p class="brand-info__intro">牙科诊所系统基于vue、element-ui构建开发，实现后台管理前端功能，提供一套更优的牙科诊所管理方案。</p>
        </div>
        <div class="login-main">
          <h3 class="login-title">注册</h3>
          <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" status-icon>
            <el-form-item prop="userName">
              <el-input v-model="dataForm.userName" placeholder="真实姓名"></el-input>
            </el-form-item>
             <el-form-item prop="idCard">
              <el-input v-model="dataForm.idCard" placeholder="身份证号"></el-input>
            </el-form-item>
            <el-form-item prop="password">
              <el-input v-model="dataForm.password" placeholder="密码"></el-input>
            </el-form-item>
            <el-form-item prop="confirm_password">
              <el-input v-model="dataForm.confirm_password" type="password" placeholder="确认密码"></el-input>
            </el-form-item>
            <el-form-item prop="age">
              <el-input v-model="dataForm.age" placeholder="年龄"></el-input>
            </el-form-item>
            <el-form-item prop="tel">
              <el-input v-model="dataForm.tel" placeholder="电话"></el-input>
            </el-form-item>
            <el-form-item prop="sex">
              性别：
              <input v-model="dataForm.sex" value="男" type="radio">
                <label for="男">男</label>                
                
              <input v-model="dataForm.sex" value="女" type="radio">
                <label for="女">女</label>                
                
            </el-form-item>
            <el-form-item>
              <router-link to="login" >
                <el-button class="login-btn-submit" type="primary" >登录</el-button>
              </router-link>
              <el-button class="login-btn-submit" type="primary" @click="dataFormSubmit()">注册</el-button>              
            </el-form-item>
        
          </el-form>
        </div>
      </div>
    </div>
  </div>
</template>

<script> 
  export default {
    data () {
      return {
        dataForm: {
          userName: '',
          idCard: '',
          password: '',
          confirm_password: '',
          age: '',
          tel: '',
          sex: '男',
          uuid: '',
          captcha: ''
        },
        dataRule: {
          userName: [
            { required: true, message: '姓名不能为空', trigger: 'blur' }
          ],
          idCard: [
            { required: true, message: '身份证号不能为空', trigger: 'blur' }
          ],
          password: [
            { required: true, message: '密码不能为空', trigger: 'blur' }
          ],
          confirm_password: [
            { required: true, message: '确认密码不能为空', trigger: 'blur' }
          ],
          age: [
            { required: true, message: '年龄不能为空', trigger: 'blur' }
          ],
          tel: [
            { required: true, message: '电话不能为空', trigger: 'blur' }
          ]
        },
        captchaPath: ''
      }
    },
    methods: {
      // 提交表单
      dataFormSubmit () {
        this.$refs['dataForm'].validate((valid) => {
          if (valid) {
            this.$http({
              url: this.$http.adornUrl('/patient/reg'),
              method: 'post',
              data: this.$http.adornData({
                'username': this.dataForm.userName,
                'idCard': this.dataForm.idCard,
                'password': this.dataForm.password,
                'confirm_password': this.dataForm.confirm_password,
                'age': this.dataForm.age,
                'tel': this.dataForm.tel,
                'sex': this.dataForm.sex,
                'uuid': this.dataForm.uuid       
              })
            }).then(({data}) => {
              if (data && data.code == 0) {
                this.$cookie.set('token', data.token)
                this.$router.replace({ name: 'login' })
              } else {
                this.getCaptcha()
                this.$message.error(data.msg)
              }
            })
          }
        })
      }
           
    }
  }
</script>

<style lang="scss">
  .site-wrapper.site-page--login {
    position: absolute;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    background-color: rgba(38, 50, 56, .6);
    overflow: hidden;
    &:before {
      position: fixed;
      top: 0;
      left: 0;
      z-index: -1;
      width: 1200px;
      height: 780px;
      content: "";
      background-image: url(~@/assets/img/login_bg.jpg);
      background-size: cover;
    }
    .site-content__wrapper {
      position: absolute;
      top: 0;
      right: 0;
      bottom: 0;
      left: 0;
      padding: 0;
      margin: 0;
      overflow-x: hidden;
      overflow-y: auto;
      background-color: transparent;
    }
    .site-content {
      min-height: 100%;
      padding: 30px 500px 30px 30px;
    }
    .brand-info {
      margin: 220px 100px 0 90px;
      color: #fff;
    }
    .brand-info__text {
      margin:  0 0 22px 0;
      font-size: 48px;
      font-weight: 400;
      text-transform : uppercase;
    }
    .brand-info__intro {
      margin: 10px 0;
      font-size: 16px;
      line-height: 1.58;
      opacity: .6;
    }
    .login-main {
      position: absolute;
      top: 0;
      right: 0;
      padding: 150px 60px 180px;
      width: 470px;
      min-height: 100%;
      background-color: #fff;
    }
    .login-title {
      font-size: 16px;
    }
    .login-captcha {
      overflow: hidden;
      > img {
        width: 100%;
        cursor: pointer;
      }
    }
    .login-btn-submit {
      width: 48%;
      margin-top: 38px;
    }
  }
</style>
