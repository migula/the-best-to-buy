<template>
  <div class="search-container">
    <!-- 头部搜索 -->
    <div class="search-box">
      <icon type="search" size="23"></icon>
      <input v-model.trim="inputData" @change="toSearch" type="text" placeholder="请输入你想找的商品">
      <button>取消</button>
    </div>
    <!-- 底部 -->
    <div class="bottom">
      <div class="history" v-if="results.length==0">
        <div class="title">
          <span>历史搜索</span>
          <i class="iconfont icon-shanchu" @click="toDelAll"></i>
        </div>
        <div class="items">
          <div
            @click="fillAndSearch(item)"
            class="item"
            v-for="(item, index) in searchList"
            :key="index"
          >
            {{item}}
            <i @click="toDelOne(index)" class="iconfont icon-shanchu"></i>
          </div>
        </div>
      </div>
      <div class="result-box" v-if="results.length!=0">
        <div class="filter-box">
          <div class="filter active">综合</div>
          <div class="filter">销量</div>
          <div class="filter">价格
            <div class="arrow-box">
              <span>▲</span>
              <span>▼</span>
            </div>
          </div>
        </div>
        <!-- 商品盒子 -->
        <div class="goods-box">
          <div class="item" v-for="(item, index) in results" :key="item.goods_id">
            <div class="left">
              <img :src="item.goods_small_logo" alt>
            </div>
            <div class="right">
              <div class="top">{{item.goods_name}}</div>
              <div class="bottom">
                ¥
                <span>{{item.goods_price}}</span>.00
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import hxios from "../../utils/index.js";
// console.log(hxios);
export default {
  data() {
    return {
      inputData: "",
      searchList: [],
      // 搜索结果
      results: []
    };
  },
  methods: {
    async toSearch() {
      // console.log(this.inputData);

      // wx.setStorage({
      //   key: 'key',
      //   // 注意:vscode自动填写的是value:'value',要用微信官方的data:'value'
      //   data: 'value'
      // });
      // 是否为空
      if (this.inputData === "") {
        return;
      }
      // 是否重复
      let index = this.searchList.indexOf(this.inputData);
      if (index != -1) {
        this.searchList.splice(index, 1);
      }
      // 限制inputData长度
      if (this.searchList.length >= 10) {
        // 删除数组第一个元素,并返回删除后的结果
        this.searchList.shift();
      }
      this.searchList.push(this.inputData);
      // 请求数据
      let res = await hxios.get({
        url: `api/public/v1/goods/search?query=${this.inputData}`
      });
      console.log(res);
      this.results = res.data.message.goods;
    },
    // 删除
    toDelAll() {
      // 暴力删除
      // this.searchList = "";
      // 温馨提示
      wx.showModal({
        title: "提示",
        content: "你确定要清空历史?O(∩_∩)O",
        // 默认是 success(){} this已经改变 使用箭头函数 固定this
        success: res => {
          if (res.confirm) {
            // console.log("用户点击确定");
            this.searchList = [];
          } else if (res.cancel) {
            // console.log("用户点击取消");
          }
        }
      });
    },
    toDelOne(index) {
      this.searchList.splice(index, 1);
    },
    // 点谁查谁
    fillAndSearch(value) {
      this.inputData = value;
      // 查询数据
      this.toSearch();
    }
  },
  // 取值
  created() {
    let res = wx.getStorageSync("history");
    //  console.log(res);
    if (res) {
      this.searchList = res;
    }
  },
  watch: {
    //  侦听 保存searchList
    searchList() {
      // console.log('fff');
      wx.setStorage({
        key: "history",
        data: this.searchList
      });
    }
  }
};
</script>

<style lang='scss'>
$uRed: #ff2d4a;
// 头部搜索
.search-box {
  display: flex;
  position: relative;
  padding: 30rpx 15rpx;
  background-color: #eeeeee;
  icon {
    position: absolute;
    left: 35rpx;
    top: 50%;
    transform: translateY(-50%);
  }
  input {
    flex: 1;
    height: 60rpx;
    padding-left: 78rpx;
    margin-right: 20rpx;
    font-size: 25rpx;
    color: black;
    background: #fff;
  }
  button {
    font-size: 28rpx;
    text-align: center;
    line-height: 60rpx;
    width: 160rpx;
    height: 60rpx;
    background-color: #eee;
    border-color: black;
    box-shadow: 0 0 10rpx yellow;
  }
}
// 底部
.bottom {
  .history {
    .title {
      display: flex;
      justify-content: space-between;
      padding: 20rpx;
      span {
        font-size: 30rpx;
      }
      i {
        text-align: center;
        font-size: 50rpx;
        color: #ddd;
      }
    }
    .items {
      display: flex;
      flex-wrap: wrap;
      margin-top: 30rpx;
      padding: 0 20rpx;
      .item {
        height: 65rpx;
        padding: 0 20rpx;
        line-height: 65rpx;
        background-color: #ddd;
        font-style: 25rpx;
        margin-right: 25rpx;
        margin-bottom: 20rpx;
        border-radius: 10rpx;
        position: relative;
        i {
          position: absolute;
          top: 0rpx;
          right: 0rpx;
          transform: translate(50%, -50%);
          color: #ccc;
        }
      }
    }
  }
  .result {
  }
}
// 搜索结果
.result-box {
  .filter-box {
    display: flex;
    align-items: center;
    height: 100rpx;
    border-bottom: 1rpx solid gray;
    .filter {
      flex: 1;
      text-align: center;
      font-size: 30rpx;
      &.active {
        color: $uRed;
      }
      &:last-child {
        display: flex;
        align-items: center;
        justify-content: center;
        .arrow-box {
          span {
            display: block;
            transform: scaleY(0.6);
          }
        }
      }
    }
  }
  // 商品盒子
  .goods-box {
    padding-left: 20rpx;
    .item {
      display: flex;
      height: 260rpx;
      border-bottom: 1rpx solid #eee;
      padding: 30rpx 0;
      box-sizing: border-box;
      .left {
        margin-right: 20rpx;
        img {
          width: 200rpx;
          height: 200rpx;
          display: block;
        }
      }
      .right {
        padding-right: 30rpx;
        display: flex;
        flex-direction: column;
        justify-content: space-between;
        .top {
          overflow: hidden;
          text-overflow: ellipsis;
          display: -webkit-box;
          -webkit-box-orient: vertical;
          -webkit-line-clamp: 2;
          font-size: 30rpx;
        }
        .bottom {
          color: $uRed;
          font-size: 22rpx;
          span {
            font-size: 32rpx;
          }
        }
      }
    }
  }
}
// 使用弹性布局约束 高度
.search-container {
  position: relative;
  padding-top: 120rpx;
  .search-box {
    position: fixed;
    width: 100%;
    left: 0;
    top: 0;
    height: 120rpx;
  }
  .result-box {
    position: relative;
    padding-top: 100rpx;
    .filter-box {
      position: fixed;
      background: white;
      width: 100%;
      left: 0;
      top: 120rpx;
    }
  }
}
</style>
