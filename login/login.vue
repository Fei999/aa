<template>
    <div class="page-login">
        <h4 class="login-head">胖胖猪H5运营系统</h4>

        <div class="login-container" id="login"> 
        
          <div class="form">
            <label class="label">Name</label>
            <div class="control">
              <input class="input" type="text" placeholder="user name"  v-model="username">
            </div>
            
            <label class="label">Password</label>
            <div class="control">
              <input class="input" type="password" placeholder="password"  v-model="password">
            </div>

            <div class="mt-4 text-center">
              <button class="login-btn" id="login-btn" @click="login" >
                <span>登录</span>
              </button>
            </div>
          </div>

        </div>
    </div>
</template>

<script>
import Qs from 'qs';
export default {
  data () {
    return {
      username:'',
      password:''
    }
  },
  methods:{
    login(){
      this.$http.post('/operationData/toTouchShow',Qs.stringify({account:this.username,password:this.password}))
      .then(response => {
        // console.log(response.data.retCode);
        if(response.data.retCode == 200){
          location.href="/#/tab";
        }else{
          alert(response.data.retMessage);
        }
      })
      .catch(err => {
        console.log(err);
      });
    }
  }
}
</script>
<style scoped>
  .page-login{
    flex: 1;
    width: 100%;
    height: auto;
    font-size: 62.5%;
    background: linear-gradient(#57cc97, 70%, rgb(54, 162, 235));
  }
  .login-head{
    padding-top: 3rem;
    color: #ffffff;
    font-size: 0.6rem;
    text-align: center;
  }
  .login-container{
    padding-top: 0.5rem;
    width:70%;
    margin-left: auto;
    margin-right: auto;
  }
  .login-container label {
    display: block;
    margin-bottom: .2rem;
    color: #ffffff;
    text-align: left;
  }
  .login-container input {
    display: block;
    width: 100%;
    margin-bottom: 0.3rem;
    padding: .5em;
    height: 40px;
    /*font-size: 1.8rem;*/
    border: 1px solid #fff;
    border-radius: 4px;
  }
  .login-container .login-btn {
    width: 40%;
    border: 1px solid #409eff;
    border-radius: 3px;
    background-color: #409eff;
    color: #ffffff;
    /*font-size: 1.6rem;*/
    height: 2.25em;
    line-height: 1.5;
    /*padding: .5rem 2rem;*/
    transition: all ease .3s;
    /*margin-top: 2rem;*/
  }
  input:-webkit-autofill, textarea:-webkit-autofill, select:-webkit-autofill {
    background-color: rgb(250, 255, 189) !important;
    background-image: none !important;
    color: rgb(0, 0, 0) !important;
  }
  .loader-box span:nth-child(1) {
  animation: loader21 0.5s -0.3s ease-in-out infinite alternate;
}
.loader-box span:nth-child(2) {
  animation: loader21 0.5s -0.15s ease-in-out infinite alternate;
}
.loader-box span:nth-child(3) {
  animation: loader21 0.5s 0s ease-in-out infinite alternate;
}
.loader-box span:nth-child(4) {
  animation: loader21 0.5s -0.15s ease-in-out infinite alternate;
}
.loader-box span:nth-child(5) {
  animation: loader21 0.5s -0.4s ease-in-out infinite alternate;
}
.loader-box span {
  width: 5px;
  height: 35px;
  background: rgb(255, 99, 132);
  border-radius: 2px;
  display: inline-block;
  margin-left: 2px;
  margin-right: 2px;
}
</style>