<template>
  <div class="button-bar">
    <div class="bar-item bar-left">
      <div>
        <i class="icon serve"></i>
        <span class="text">客服</span>
      </div>
      <div>
        <i class="icon shop"></i>
        <span class="text">店铺</span>
      </div>
      <div>
        <i class="icon select"></i>
        <span class="text">收藏</span>
      </div>
    </div>
    <div class="bar-item bar-rigth">
      <div class="cart" @click="addToCart">加入购物车</div>
      <div class="buy">购买</div>
    </div>
  </div>
</template>

<script>
export default {
  name: "DetailButtonBar"
}
</script>

<style scoped>
  .button-bar {
    height: 49px;
    background-color: red;

    position: relative;
    bottom: 49px;

    /* position: fixed;
    left: 0;
    right: 0;
    bottom: 0; */
  }

  
</style>