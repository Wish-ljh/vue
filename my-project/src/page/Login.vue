<template>
<div>
  <SignHeader>账号登录</SignHeader>
  <SignMain>
    <div class="title">账号登录</div>
      <div>
          <div class="input-login">
              <el-input placeholder="注册邮箱/客服账号" v-model="accountInput">
                  <template slot="prepend">账号</template>
              </el-input>
          </div>
          <div class="input-login">
              <el-input placeholder="6-16字符" v-model="passwordInput" type="password">
                  <template slot="prepend">密码</template>
              </el-input>
          </div>
          <div class="input-login">
              <el-input placeholder="请输入域名" v-model="domainInput">
                  <template slot="prepend">域名</template>
                  <template slot="append">.qiyukf.com</template>
              </el-input>
          </div>
      </div>
      <el-button @click="loginClick" type="primary" :loading="loading" :disabled="disabled" style="font-size: 16px">登录</el-button>
      <div class="group-forget">
        <el-checkbox v-model="checked">记住密码</el-checkbox>
        <el-popover
          title="忘记密码？"
          placement="top-start"
          width="340"
          trigger="hover"
          content="忘记密码请联系企业超级管理员重置密码超级管理员忘记密码可尝试">
          <span slot="reference">无法登录</span>
        </el-popover>

      </div>
  </SignMain>
  <footer>还没有账号? <router-link class="register" to="/regist">立即注册</router-link></footer>
  <Footer />
</div>

</template>

<script>
import Footer from "../components/Footer";
import SignHeader from "../components/SignHeader";
import SignMain from "../components/SignMain";
  export default {
    components:{Footer,SignHeader,SignMain},
    data () {
      return {
        loading:false,
        accountInput:"",
        passwordInput:"",
        domainInput:"",
        checked:false,
      }
    },
    methods:{
        loginClick(){
          const {accountInput,passwordInput,domainInput,checked} = this;
          const data = {account:accountInput,password:passwordInput,companyDomain:domainInput,remember_me:checked}
          // console.log(data);
          this.loading = true;
          this.$http.post(this.rootUrl+'/user/login', data).then(data => {
            console.log(data.data);
            if (data.data.code == '200'){
              const token = data.data.result.token;
              sessionStorage.setItem("token",token);
              this.$router.push('/index');
            }else {
              this.$message.error('帐号、密码或域名有误!');
              this.loading = false;
            }
          }).catch(function (err) {
            console.log(err);
              this.loading = false;
          });
        }
    },
    computed:{
      disabled(){
        const {accountInput,passwordInput,domainInput} = this;
        if(!accountInput||!passwordInput||!domainInput)return true;
        return false;
      }
    },

  }
</script>

<style scoped>

  .title{
    font-size: 34px;
    color: #373d40;
    line-height: 1;
    padding: 35px 0;
    text-align: center;
  }
 /* .input{
    display: flex;
    align-items: center;
    padding-bottom: 30px;
  }*/
  .input>div:nth-of-type(1){
    width: 20%;
  }
  .input>div:nth-of-type(2){
    width: 80%;
  }
  .el-button{
    width: 100%;
    margin-top: 30px;
  }
  .group-forget{
    display: flex;
    justify-content: space-between;
    margin-top: 35px;
  }
  .group-forget span{
    color: #4291e6;
    font-size: 14px;
  }
  footer{
    text-align: center;
    margin-bottom: 30px;
    font-size: 14px;
    color: #9898a2;
    line-height: 20px;
  }
  .register{
    text-decoration: none;
    color: #4291e6;
  }
  .input-login{
      margin-top: 20px;
  }
</style>
