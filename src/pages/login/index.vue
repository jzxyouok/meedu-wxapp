<template>
  <div class="login">
    <p class="login-title">MeEdu Login</p>
    <view class="section">
      <input  v-model="form.account" placeholder-class="input-holder" placeholder="请输入您的账号"/>
    </view>
    <view class="section">
      <input type="password" v-model="form.password" placeholder-class="input-holder" :password="isPassword" placeholder="请输入您的密码"/>
    </view>
    <button class="btn" @click="login">登录</button>
    <button class="btn btn-primary" @click="toIndex">游客登录</button>
    <p class="login-text">登陆/注册即视为同意<span style="color: #8ECEF4;">meEdu协议</span></p>
    <!-- <p class="login-text" @click="goRegister">没有meedu账号？立即注册</p> -->
  </div>
</template>

<script>
import { formatTime } from '@/utils/index'
import card from '@/components/card'

export default {
  components: {
    card
  },

  data () {
    return {
      isPassword: true,
      logs: [],
      form: {
        account: '',
        password: ''
      }
    }
  },
  methods: {
    toIndex () {
      wx.switchTab({
        url: '../tabBar/course/main'
      })
    },
    goRegister () {
      wx.navigateTo({
        url: '../register/main'
      })
    },
    ckeckLogin () {},
    login () {
      const data = {
        scope: '',
        grant_type: 'password',
        client_id: '2',
        client_secret: 'G8hmzjkkJBl9lPwF45pBgO1AJSM5XolpbPNFR9k7',
        username: this.form.account || '',
        password: this.form.password || ''
      }
      if (!this.form.account) {
        wx.showToast({
          title: '账号不能为空',
          icon: 'none',
          mask: true
        })
        return false
      }
      if (!this.form.password) {
        wx.showToast({
          title: '密码不能为空',
          icon: 'none',
          mask: true
        })
        return false
      }
      this.$http.user.login(data).then(res => {
        if (res.expires_in) {
          console.log(new Date())
        }
        if (res.access_token) wx.setStorageSync('access_token', res.access_token)
        wx.switchTab({
          url: '../tabBar/course/main'
        })
      }).catch(err => {
        console.log('catch err', err)
        wx.switchTab({
          url: '../tabBar/course/main'
        })
      })
    }
  },
  created () {
    if (wx.getStorageSync('access_token')) {
      wx.switchTab({
        url: '../tabBar/course/main'
      })
    }
    const logs = (wx.getStorageSync('logs') || [])
    this.logs = logs.map(log => formatTime(new Date(log)))
  }
}
</script>

<style lang="less">
.login{
  position: relative;
  top: 0;
  left: 0;
  padding: 0 45px;
  p{
    text-align: center;
  }
  &-title{
    color: #111111;
    font-size: 20px;
    padding: 40px 0 30px;
  }
  .section{
    input{
      width: 100%;
      height: auto;
      font-size: 14px;
      border-bottom: 1px solid #666;
      margin-bottom: 30px;
    }
    .input-holder{
      color: #777777;
      font-size: 14px;
    }
  }
  .btn{
    color: #fff;
    background: #373737;
    margin:10px 0 20px;
  }
  .btn-primary{
    color: #373737;
    background: #fff;
  }
  .login-text{
    line-height: 2.0;
    font-size: 12px;
    color: #777777;
    text-decoration: none;
    text-align: center;
  }
}

</style>
