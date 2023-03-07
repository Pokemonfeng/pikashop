<template>
<view class="goods-item">
  <!-- 左侧盒子 -->
  <view class="goods-item-left">
    <radio color="#C00000" :checked="goods.goods_state" @click="radioClickHandler" v-if="showRadio" ></radio>
    <image class="goods-pic" :src="goods.goods_small_logo || defaultPic"></image>
  </view>
  <!-- 右侧盒子 -->
   <view class="goods-item-right">
    <uni-icons type="trash-filled" size="14"  @click="goodsRemove" v-if="showRemove" class="goods-remove"></uni-icons>
    <view class="goods-name">{{goods.goods_name}}</view>
    <view class="goods-info-box">
      <view class="goods-price">￥ {{goods.goods_price | tofixed}}</view>
      <uni-number-box  :min="1" :value="goods.goods_count" v-if="showNum" @change="numClickHandler"></uni-number-box>
    </view>
   </view>
</view>
</template>
 
<script>

export default {
  name: 'MyGoods',
  props: {
    goods: {
      type: Object,
      default: {

      }
    },
    showRadio: {
      type: Boolean,
      default: false
    },
    showNum: {
      type: Boolean,
      default: false
    },
    showRemove: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      defaultPic: '../../static/images/暂无信息数据-缺省页.png',
      
    }
  },
  methods: {
    radioClickHandler(e) {
      this.$emit('radio-change', {
        goods_id: this.goods.goods_id,
        goods_state: !this.goods.goods_state
      })
    },

    numClickHandler(val) {
      this.$emit('num-change', {
        goods_id: this.goods.goods_id,
        goods_count: +val
      })
    },
    goodsRemove(rem) {
      this.$emit('goods-remove', {
        goods_id: this.goods.goods_id
      })
    }
  },
  filters: {
    tofixed(num) {
      return Number(num).toFixed(2)
    }
  }
}
</script>

<style lang="scss" scoped>

.goods-item {
  width: 750rpx;
  box-sizing: border-box;
  display: flex;
  padding: 10px 5px;
  border-bottom: 1px solid #e0e0e0;

  .goods-item-left {
    margin-right: 5px;
    display: flex;
    justify-content: space-between;
    align-items: center;



    .goods-pic {
      width: 100px;
      height: 100px;
      display: block;
    }
  }

  .goods-item-right {
    display: flex;
    flex: 1;
    flex-direction: column;
    justify-content: space-between;

    .goods-name {
      font-size: 13px;
    }

    .goods-remove {
      position: relative;
      right: 0;
      top: 0;
    }

    .goods-info-box {
      display: flex;
      justify-content: space-between;
      align-items: center;

      .goods-price {
        color: #C00000;
        font-size: 16px;
        font-weight: 700;
      }
    }

    
  }

}

</style>



