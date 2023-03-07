<template>
  <view class="login-container">
    <uni-icons
      type="contact-filled"
      color="#AFAFAF"
      size="100"
    />

    <button type="primary" class="btn-login" open-type="getUserInfo" @getuserinfo="getUserinfo">一键登录</button>
    <text class="tips-text">登录后尽享更多权益</text>
  </view>
</template>

<script>
import { mapMutations, mapState } from 'vuex' 
export default {
  name: 'MyLogin',
  data() {
    return {

    }
  },
  computed: {
    ...mapState('m_user',['redirectInfo'])
  },
  methods: {
    ...mapMutations('m_user',['updateUserInfo', 'updateToken', 'upadreRedirectInfo']),
      getUserinfo(e) {
        if(e.detail.errMsg === 'getUserInfo:fail auth deny') return uni.$showMsg('您取消了登录授权！')
      // console.log(e.detail.userInfo)
      this.updateUserInfo(e.detail.userInfo)

      this.getToken(e.detail)
      // console.log(e.detail)
    },

    async getToken(info) { 
      const [err, res] = await uni.login().catch(err => err)
      // console.log(res.errMsg);

      if(err || res.errMsg !== 'login:ok') return uni.$showMsg('登录失败!')
      console.log(info)

      const query = {
        code: res.code,
        encryptedData: info.encryptedData,
        iv: info.iv,
        rawData: info.rawData,
        signature: info.signature
      }
      // console.log(query)
      // const { data: loginResult } = await uni.$http.post('/api/public/v1/users/wxlogin', query)
      // console.log(loginResult);
      // if (loginResult.meta.status !== 200) return uni.$showMsg('登录失败！')
      //  uni.$showMsg('登录成功!')

      //  this.updateToken(loginResult.message.token)
      this.updateToken('czf')
      this.navigateBack()
    },
    navigateBack() {
      if(this.redirectInfo && this.redirectInfo.openType === 'switchTab') {
        uni.switchTab({
          url: this.redirectInfo.from,
          complete: () => {
            this.upadreRedirectInfo(null)
          }
        
        })
      }
    }
  }
  
}
</script>

<style lang="scss">

.login-container {
  height: 750rpx;
  background-color: #F8F8F8;
  display: flex;
  align-items: center;
  flex-direction: column;
  justify-content: center;
  position: relative;
  overflow: hidden;

  &::after {
    content: ' ';
    display: block;
    width: 100%;
    height: 40px;
    background-color: white;
    position: absolute;
    bottom: 0;
    left: 0;
    border-radius: 100%;
    transform: translateY(50%); 
  }


  .btn-login {
    width: 90%;
    background-color: #C00000;
    border-radius: 100px;
    margin: 15px 0;
  }

  .tips-text {
    font-size: 12px;
    color: gray;
  }
}

</style>