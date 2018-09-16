<template>
  <div class="backlogin">
    <div class="login-wrap">
      <div class="title">后台登录</div>
      <div>
        <input class="myinput" type="text" placeholder="手机号/用户名" v-model="username">
      </div>
      <div>
        <input @keyup.13="login" class="myinput" type="password" placeholder="口令" v-model="password">
      </div>
      <div class="login-other">
        <a href="javascript:;void (0)">找回密码</a>
        <input type="checkbox" id="remember">
        <label for="remember">记住我</label>
      </div>
      <button :disabled="disablebtn" class="login" @click="login">{{ loginText }}</button>
    </div>
  </div>
</template>

<script>
  export default {
    name: "login",
    data() {
      return {
        username: "",
        password: "",
        disablebtn: false,
        loginText: "登录"
      }
    },
    methods: {
      login() {
        var that = this
        this.disablebtn = true
        this.loginText = "登录中..."
        //this.$reqs就访问到了main.js中绑定的axios
        this.$reqs.post("/users/login",{
          username: this.username,
          password: this.password
        }).then(result => {
          console.log(result)
          if (result.data.err) {
            alert(result.data.err)
          } else {
            that.$router.push({ path:'/backIndex/indexContent'})
          }
          that.disablebtn = false
          that.loginText = "登录"
        }).catch(error => {
          that.disablebtn = false
          that.loginText = "登录"
        })
      }
    }
  }
</script>

<style scoped>
  .header {
    height: 60px;
    box-shadow: 0 1px 5px rgba(13, 62, 73, .2);
  }

  .header img {
    width: 170px;
    margin-top: 12px;
    margin-left: 15px;
    float: left;
  }

  .header span {
    float: left;
    color: #566a80;
    margin: 21px 0 0 20px;
  }

  .login-wrap {
    width: 320px;
    margin: 50px auto;
  }

  .login-wrap .myinput {
    width: 100%;
    border: 1px solid #cad3de;
    height: 40px;
    line-height: 40px;
    margin: 5px 0 10px;
    border-radius: 3px;
    padding: 0 10px;
    outline: none;
    box-sizing: border-box;
  }

  .login-wrap .myinput:focus {
    border: 1px solid #4289dc;
  }
  .login-other {
    overflow: hidden;
  }
  .login-other a {
    float: right;
    color: #727f8f;
  }
  .login-other a:hover {
    color: #273444;
  }
  .login-other input, .login-other label {
    float: left;
    color: #727f8f;
  }
  .login-other input {
    margin: 4px 5px 0 0;
  }
  .login {
    box-sizing: border-box;
    border: none 0;
    height: 44px;
    line-height: 44px;
    width: 100%;
    background-color: #4187db;
    font-size: 16px;
    border-radius: 3px;
    margin-right: 40px;
    transition: all .5s ease;
    cursor: pointer;
    outline: none;
    color: #fff;
    margin-top: 15px;
  }
  .login:hover {
    background-color: #2668b5;
  }
  .login[disabled] {
    opacity: 0.8;
  }
  .login[disabled]:hover {
    background:#4187db;
  }
  .title {
    color: #273444;
    font-size: 1.5em;
    text-align: center;
    margin: 0 0 20px 0;
  }

  @media only screen and (max-width: 768px) {
    .login-wrap {
      width: 280px;
      margin: 50px auto;
    }
  }
</style>
