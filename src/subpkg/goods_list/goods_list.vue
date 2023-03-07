<template>
  <view>
    <view class="goods-list">
      <view v-for="(goods, i) in goodList" :key="i" @click="gotoDetail(goods)">
        <my-goods :goods="goods"></my-goods>
      </view>
    </view>
  </view>
</template>

<script>
import MyGoods from '../../components/my-goods/my-goods.vue'

export default {
  data() {
    return {
      queryObj: {
        // 请求参数对象
        query: '',
        cid: '',
        pagenum: 1,
        pagesize: 10

      },

      goodList: [],
      total: 0,
      defaultPic: '../../static/images/暂无信息数据-缺省页.png',
      isloading: false
    }
  },
  components: {
    MyGoods
  },

  onLoad(options) {
    this.queryObj.query = options.query || ''
    this.queryObj.cid = options.cid || ''

    this.getGoodsList()
  },

  methods: {

    async getGoodsList(cb) {
      this.isloading = true
      const {data: res} = await uni.$http.get('/api/public/v1/goods/search', this.queryObj)
      this.isloading = false
      cb && cb()
      if(res.meta.status !== 200) return uni.$showMsg()

      this.goodList = [...this.goodList, ...res.message.goods]
      // console.log(res);
      this.total = res.message.total
    },

    gotoDetail(goods) {
      console.log(goods);
      uni.navigateTo({ url: '/subpkg/goods_detail/goods_detail?goods_id=' + goods.goods_id })
    }
  },

  onReachBottom() {
    if(this.queryObj.pagenum * this.queryObj.pagesize >= this.total) return uni.$showMsg('数据加载完毕！')

    if(this.isloading == true) return
    // 让页码值自增 +1
    this.queryObj.pagenum += 1
    // 重新获取列表数据
    this.getGoodsList()
  },

  onPullDownRefresh() {
    // 重置关键数据
    this.queryObj.pagenum = 1
    this.total = 0
    this.isloading = false
    this.goodList = []

    // 重新发起数据请求
    this.getGoodsList(() => uni.stopPullDownRefresh())
  }

}
</script>

<style lang="scss">


</style>