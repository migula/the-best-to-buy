<template>
  <div>
   <div class="index-container">
     <div class="search-box">
       <input type="text" placeholder="搜索">
       <icon  type="search" size="12"> </icon>
     </div>
   </div>
  </div>
</template>

<script>

export default {
  
}
</script>

<style lang="scss">
$ured:#ff2d4a;
.search-box{
  background-color: $ured;
  padding: 20rpx 16rpx;
  box-sizing: border-box;
  width:100%;
  position: fixed;

  input{
    display: block;
    width: 100%;
    box-sizing: border-box;
    padding-left: 376rpx;
    background-color: white;
    font-size: 24rpx;
    height: 60rpx;
    border-radius: 10rpx;
  }
  icon{
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%,-50%);
  }
}
</style>
