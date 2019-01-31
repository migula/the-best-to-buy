<template>
  <div class="category-container">
    <!-- 抽取的组件名 -->
    <searchBox></searchBox>
    <div class="scroll-box">
      <!-- 左边盒子 -->
      <scroll-view class="left" scroll-y scroll-with-animation>
        <!-- 渲染数据 -->
        <ul>
            <!-- @click="index==i" 点击高亮-->
          <li
            :class="{actived:index===i}"
            v-for="(item, i) in categoryList"
            :key="item.cat_id"
            @click="index=i"
          >{{item.cat_name}}</li>
        </ul>
      </scroll-view>
      <!-- 右边盒子 -->
      <scroll-view class="right" scroll-y scroll-with-animation></scroll-view>
    </div>
  </div>
</template>

<script>
// 导入抽取的组件
import searchBox from "../../components/searchBox";
// 导入hxios
import hxios from "../../utils/index.js";

export default {
  data() {
    return {
      categoryList: [],
      index:0
    };
  },
  // 注册组件
  components:{
    searchBox
  },
  // 获取数据
  async created() {
    let res = await hxios.get({
      url: "api/public/v1/categories"
    });
    // console.log(res);
    this.categoryList = res.data.message;
  }
};
</script>

<style lang='scss'>
$uRed:#ff2d4d;
page{
  height: 100%;
}
.category-container {
  padding-top: 100rpx;
  box-sizing: border-box;
  height: 100%;
  .scroll-box {
    display: flex;
    height: 100%;
    scroll-view {
      // 竖直方向滚动 需要设置固定的高度
      height: 100%;
    }
    .left {
      width: 200rpx;
      ul {
        li {
          font-size: 26rpx;
          text-align: center;
          height: 100rpx;
          line-height: 100rpx;
          background-color: #aaa;
          &.actived {
            font-weight: 900;
            color: $uRed;
            position: relative;
            background-color: white;
            &::before {
              content: "";
              position: absolute;
              width: 10rpx;
              height: 60rpx;
              background-color: $uRed;
              left: 0;
              top: 20rpx;
            }
          }
        }
      }
    }
    .right {
      flex: 1;
      background-color: skyblue;
    }
  }
}
</style>
