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
    
     <!-- 楼层区域 -->
    <div class="floor-container">
      <div class="floor" v-for="item in floorList">
        <!-- 顶部 -->
        <div class="top">
          <img :src="item.floor_title.image_src" alt>
          <h3>{{item.floor_title.name}}</h3>
        </div>
        <!-- 底部 -->
        <div class="bottom">
          <img v-for="(it,i)  in item.product_list" :src="it.image_src" alt>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import hxios from "../../utils/index.js";
export default {
  data() {
    return {
      swiperList: [],
      categoryList: [],
      floorList:[],

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
      // 获取楼层数据
    let floorRes = await hxios.get({
      url: "api/public/v1/home/floordata"
    });
    console.log(floorRes);
    this.floorList = floorRes.data.message;

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
// 楼层数据
.floor-container {
  .floor {
    .top {
      padding-top: 30rpx;
      padding-bottom: 30rpx;
      padding-left: 15rpx;
      position: relative;
      height: 90rpx;
      box-sizing: border-box;
      h3 {
        color: #ff7b94;
        position: absolute;
        left: 30rpx;
        top: 50%;
        transform: translateY(-50%);
      }
      img {
        position: absolute;
        height: 90rpx;
        left: 0;
        top: 0;
        display: block;
        width: 100%;
      }
    }
    .bottom {
      padding: 20rpx 0 0 16rpx;
      overflow: hidden;
      img {
        display: block;
        width: 33.333%;
        float: left;
        width: 230rpx;
        height: 190rpx;
        margin-right: 10rpx;
        &:first-child {
          width: 230rpx;
          height: 390rpx;
        }
        &:nth-child(2) {
          margin-bottom: 10rpx;
        }
        &:nth-child(3) {
          margin-bottom: 10rpx;
        }
      }
    }
  }
}
</style>
