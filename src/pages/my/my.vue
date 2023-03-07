<template>
  <view class="my-container">
    <my-login v-if="!token"></my-login>

    <my-userinfo v-else></my-userinfo>
  </view>
</template>

<script>
import badgeMix from '../../mixins/tabbar-badge'
import MyLogin from '../../components/my-login/MyLogin.vue'
import MyUserinfo from '../../components/my-userinfo/MyUserinfo.vue'

import { mapState } from 'vuex'

export default {
  mixins: [badgeMix],
  data() {
    return {
      
    }
  },
  computed: {
    ...mapState('m_user', ['token']),

  },
  components: {
    MyLogin,
    MyUserinfo
  }
}
</script>

<style lang="scss">
page,
.my-container {
  height: 100%;
}

</style>