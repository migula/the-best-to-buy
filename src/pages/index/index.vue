<template>
  <div>
    <!-- 顶部搜索 -->
    <div class="index-container">
      <div class="search-box">
        <input type="text" placeholder="搜索">
        <icon type="search" size="12"></icon>
      </div>
    </div>
    <!-- 轮播图 -->
    <div class="swiper-container">
      <swiper indicator-dots autoplay >
        <swiper-item v-for="item in swiperList" :key="item.image_src">
          <img :src="item.image_src" class="slide-image" width="355" height="150">
        </swiper-item>
      </swiper>
    </div>
    <!--分类 -->
    <div class="category-container">
      <div class="item" v-for="item in categoryList">
        <img class="title" :src="item.image_src">
        <p>{{item.name}}</p>
      </div>
    </div>
    <!--楼层 -->
  </div>
</template>

<script>
import hxios from "../../utils/index.js";
export default {
  data() {
    return {
      swiperList: [],
      categoryList: []
    };
  },
  async created() {
    let swiperRes = await hxios.get({
      url: "api/public/v1/home/swiperdata"
    });
    // console.log(swiperRes);
    this.swiperList = swiperRes.data.message;
    let navigateRes = await hxios.get({
      url: "api/public/v1/home/catitems"
    });
    this.categoryList = navigateRes.data.message;
  }
};
</script>

<style lang="scss">
$ured: #ff2d4a;
.search-box {
  background-color: $ured;
  padding: 20rpx 16rpx;
  box-sizing: border-box;
  width: 100%;
  position: fixed;

  input {
    display: block;
    width: 100%;
    box-sizing: border-box;
    padding-left: 376rpx;
    background-color: white;
    font-size: 24rpx;
    height: 60rpx;
    border-radius: 10rpx;
  }
  icon {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
  }
}
// 轮播
.swiper-container {
  swiper {
    height: 340rpx;
    swiper-item {
      height: 100%;
    }
  }
  img {
    display: block;
    width: 100%;
    height: 100%;
  }
}
// 分类列表样式
.category-container {
  display: flex;
  padding-top: 24rpx;
  padding: 20rpx;
  .item {
    flex: 1 ;
    img {
      width: 120rpx;
      height: 120rpx;
      margin:0 auto;
    }
    p {
      text-align: center;
      font-size: 24rpx;
      margin-top: 10rpx;
    }
  }
}
</style>
