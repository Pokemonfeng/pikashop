<template>
  <view class="cart-container" v-if="cart.length !== 0">
    <!-- 收获地址组件 -->
    <my-address></my-address>
    <!-- 商品列表的标题区域 -->
    <view class="cart-title">
      <!-- 左侧图标 -->
      <uni-icons
        type="shop"
        color=""
        size="18"
      />
      <!-- 右侧文本 -->
      <text class="cart-title-text">购物车</text>
    </view>

    <!-- 循环渲染购物车中的商品信息 -->
    <!-- <uni-swipe-action> -->
      <block v-for="(goods, i) in cart" :key="i">
        <!-- <uni-swipe-action-item :right-options="options"  @click="swipeActionClickHandler(goods)" > -->
          <my-goods :goods="goods" :show-num="true" :show-radio="true"  :showRemove="true" @radio-change="radioChangeHandler" @num-change="numChangeHandler" @goods-remove="swipeActionClickHandler"></my-goods>
        <!-- </uni-swipe-action-item> -->
      </block>

    <!-- </uni-swipe-action> -->

    <!-- 结算 -->
    <my-settle></my-settle>


  </view>

  <!-- 空白购物车区域 -->
  <view class="empty-cart" v-else>
    <image src="../../static/images/暂无信息数据-缺省页.png" class="empty-img"></image>
    <text class="tip-text">空空如也~</text>
  </view>
</template>

<script>
import badgeMix from '../../mixins/tabbar-badge'
import { mapState, mapMutations } from 'vuex'

import MyAddress from '../../components/my-address/MyAddress.vue'
import MyGoods from '../../components/my-goods/my-goods.vue'
import MySettle from '../../components/my-settle/MySettle.vue'

export default {
  mixins: [badgeMix],
  computed: {
    ...mapState('m_cart', ['cart'])
  },
  components: {
    MyGoods,
    MyAddress,
    MySettle
  },
  data() {
    return {
      options: [{
      text: '删除', // 显示的文本内容
      style: {
        backgroundColor: '#C00000' // 按钮的背景颜色
      }
    }]
    }
  },
  methods: {
    ...mapMutations('m_cart', ['updateGoodsState', 'updateGoodsCount', 'removeGoodsById']),
    radioChangeHandler(e) {
      console.log(e);
      this.updateGoodsState(e)
    },
    numChangeHandler(val) {
      // console.log(val);
      this.updateGoodsCount(val)
    },
    swipeActionClickHandler(goods) {
    this.removeGoodsById(goods.goods_id)
    }


  }
  
}
</script>

<style lang="scss">

.cart-container {
  padding-bottom: 50px;
}

.cart-title {
  height: 40px;
  display: flex;
  align-items: center;
  padding-left: 5px;
  border-bottom: 1px solid #EFEFEF;
}

.cart-title-text {
  margin-left: 10px;
}


.slot-button {
  background-color: #E50012;
  display: flex;
  width: 150rpx;
  justify-content: center;
  align-items: center;
    .slot-button-text {  
      color: #ddd;  
      font-size: 14px;  
    }
  }

  .empty-cart {
    display: flex;
    flex-direction: column;
    align-items: center;
    padding-top: 150px;

    .empty-img {
      width: 100x;
      height: 120px;
    }

    .tip-text {
      font-size: 14px;
      color: gray;
      margin-top: 15px;
    }
  }


</style>