<template>
  <div class="loginbox">
    <el-form ref="AccountFrom" :model="account" :rules="rules" label-position="left" label-width="0px" class="demo-ruleForm login-container">
      <h3 class="title">管理员登录</h3>
      <el-form-item prop="username">
        <el-input type="text" v-model="account.username" auto-complete="off" placeholder="请输入账号" clearable></el-input>
      </el-form-item>
      <el-form-item prop="pwd">
        <el-input type="password" v-model="account.pwd" auto-complete="off" placeholder="请输入密码" @keyup.enter.native="handleLogin" clearable>
        </el-input>
      </el-form-item>
      <!-- <el-checkbox v-model="checked" class="remember">记住密码</el-checkbox> -->
      <el-form-item style="width:100%;">
        <el-button type="primary" style="width:100%;"  @click.native.prevent="handleLogin" :loading="loading" :disabled="disa">登录</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
  import API from '../api/api_user';

  export default {
    data() {
      return {
        loading: false,
        account: {
          username: '',
          pwd: ''
        },
        disa:false,
        rules: {
          username: [
            {required: true, message: '请输入账号', trigger: 'blur'},
            //{ validator: validaePass }
          ],
          pwd: [
            {required: true, message: '请输入密码', trigger: 'blur'},
            //{ validator: validaePass2 }
          ]
        },
        checked: false
      };
    },
    methods: {
      handleLogin() {
        let that = this;
        this.$refs.AccountFrom.validate((valid) => {
          if (valid) {
            this.loading = true;
            let loginParams = {userAccount: this.account.username, userPassword: this.account.pwd};
            API.login(loginParams).then(function (result) {
              that.loading = false;
              if (result && result.status === "101") {
                localStorage.setItem('access-user', JSON.stringify(result.data));
//                that.$store.commit('SET_ROUTERS', user.permissions)
//                that.$router.addRoutes(that.$store.getters.addRouters);
//                that.$router.options.routes = that.$store.getters.routers;
                that.$router.push({path: '/'});
              } else {
                that.$message.error({showClose: true, message: '登录失败！账号或密码错误', duration: 2000});
              }
            }, function (err) {
              that.loading = false;
              // err.toString()
              that.$message.error({showClose: true, message: "连接服务器失败！请稍后再试", duration: 2000});
            }).catch(function (error) {
              that.loading = false;
              that.$message.error({showClose: true, message: '请求出现异常', duration: 2000});
            });
          }
        });
      }
    },
    computed: {
      
    }
  }
</script>
<style>
  body {
    /* background: #DFE9FB; */
    background: url('../../static/loginbg.jpg') no-repeat;
    background-size: 100% 100%;
  };
</style>
<style lang="scss" scoped>
  .login-container {
    /*box-shadow: 0 0px 8px 0 rgba(0, 0, 0, 0.06), 0 1px 0px 0 rgba(0, 0, 0, 0.02);*/
    -webkit-border-radius: 5px;
    border-radius: 5px;
    -moz-border-radius: 5px;
    background-clip: padding-box;
    // margin: 160px auto;
    width: 350px;
    padding: 35px 35px 15px 35px;
    background: #fff;
    border: 1px solid #eaeaea;
    box-shadow: 0 0 15px #cac6c6;
    background: -ms-linear-gradient(top, #ace, #00C1DE); /* IE 10 */
    background: -moz-linear-gradient(top, #ace, #00C1DE); /*火狐*/
    background: -webkit-gradient(linear, 0% 0%, 0% 100%, from(#ace), to(#00C1DE)); /*谷歌*/
    background: -webkit-linear-gradient(top, #ace, #00C1DE); /*Safari5.1 Chrome 10+*/
    background: -o-linear-gradient(top,#ace, #00C1DE); /*Opera 11.10+*/

    position:absolute; 
    top:50%; 
    left:50%;
    margin-left:-211px; 
    margin-top:-151.5px;

    .title {
      margin: 0px auto 40px auto;
      text-align: center;
      color: #505458;
    }
    .remember {
      margin: 0px 0px 35px 0px;
    }
    .loginbox{
      

    }
  }
</style>
